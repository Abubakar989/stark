
export function getPlate(n: number): string {
  if (n < 0) {
    throw new Error("Index must be non-negative");
  }

  for (let k = 0; k <= 6; k++) {
    const numCount = 10 ** (6 - k);     
    const letterCount = 26 ** k;        
    const groupSize = numCount * letterCount;

    // Check if nth element falls in this group
    if (n < groupSize) {
      const numIndex = n % numCount;
      const letterIndex = Math.floor(n / numCount);

      const numPart = numIndex
        .toString()
        .padStart(6 - k, "0");

      const letterPart = toBase26(letterIndex, k);

      return numPart + letterPart;
    }

    n -= groupSize;
  }

  throw new Error("Index out of range");
}


function toBase26(num: number, length: number): string {
  let result = "";

  for (let i = 0; i < length; i++) {
    const remainder = num % 26;
    result = String.fromCharCode(65 + remainder) + result;
    num = Math.floor(num / 26);
  }

  return result;
}
