## 46. 全排列

```ts
function permute(nums: number[]): number[][] {
    const result: number[][] = [];
    const num1: number[] = [];
    let n = nums.length;
    function backtrack() {
        if (num1.length === n) {
            result.push([...num1])
        }
        for (let i = 0; i < n; i++) {
            if (num1.includes(nums[i])) continue;
            num1.push(nums[i]);
            backtrack();
            num1.pop();
        }

    }

    backtrack();
    return result;
}
```

