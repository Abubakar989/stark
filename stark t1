type FetchFn<T> = (url: string) => Promise<T>;

export async function fetchWithConcurrency<T>(
  urls: string[],
  maxConcurrency: number,
  fetchFn: FetchFn<T>
): Promise<T[]> {
  const results: T[] = new Array(urls.length);
  let nextIndex = 0;

  async function worker(): Promise<void> {
    while (true) {
      const current = nextIndex++;
      if (current >= urls.length) return;

      results[current] = await fetchFn(urls[current]);
    }
  }

  const workers: Promise<void>[] = Array.from(
    { length: Math.min(maxConcurrency, urls.length) },
    () => worker()
  );

  await Promise.all(workers);

  return results;
}


function createMockFetch() {
  let active = 0;
  let maxActive = 0;

  return {
    fetch: async (url: string): Promise<string> => {
      active++;
      maxActive = Math.max(maxActive, active);

      await new Promise((r) =>
        setTimeout(r, Math.random() * 100)
      );

      active--;
      return `response:${url}`;
    },
    getMaxActive: () => maxActive
  };
}

(async () => {
  const urls = ["a", "b", "c", "d"];
  const maxConcurrency = 2;

  const mock = createMockFetch();

  const results = await fetchWithConcurrency<string>(
    urls,
    maxConcurrency,
    mock.fetch
  );

  console.assert(results.length === urls.length);
  console.assert(
    results.every((res, i) => res === `response:${urls[i]}`)
  );
  console.assert(mock.getMaxActive() <= maxConcurrency);

  console.log("All tests passed!");
})();
