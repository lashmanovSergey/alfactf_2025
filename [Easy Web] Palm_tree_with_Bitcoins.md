### Task "Palm tree with Bitcoins"
Difficulty: Easy
Category: Web

#### Analysis:

Here we see shop web-application, that enables us to buy some products. Back-end of the server is written on javascript and given to us, so we can examine code.
Lets check the following function:

```javascript
function parseSlice(sliceCode) {
  if (typeof sliceCode !== "string") return null;

  const parts = sliceCode.split("/");
  if (parts.length !== 2) return null;

  const left = parts[0].trim();
  const right = parts[1].trim();

  const numN = Number(left);
  const denN = Number(right);

  if (!Number.isFinite(numN) || !Number.isFinite(denN)) return null;
  if (!Number.isInteger(numN) || !Number.isInteger(denN)) return null;
  if (numN < 0) return null;

  return { code: `${numN}/${denN}`, num: numN, den: denN };
}
```

We can control this input. But there is some restrictions about our input. The input does not allowed to be negative or to be finite big or small. But they missed one moment, I mean, 
they do not except division on zero. In javascript, if you divide 1 over 0 you get Infinity and if you divide 1 over -0 you get -Infinity. The total basket is calculated as a sum of prices of each product. 
What will be if you sum Infinity and -Infinity in javascript? Yep, you will get NaN, an empty value.

But we know, that any number is not greater or smaller than NaN. It's simpy equals to false. There is next misconfiguration:
```javascript
if (!(session.balanceSats < total)){...}
```

The application should check balance as (session.balanceSats > total), but it does it in the opposite way. so it allows to make purchase, because:
1) NaN < total - false
2) !(false) - true
and success, we did our purchase and got flag in the response!

#### Exploitation:
##### step 1 (make basket price equal to NaN):
<img width="620" height="206" alt="image" src="https://github.com/user-attachments/assets/4817c5de-2144-4e0d-aa7b-284dbd3994b1" />
<img width="617" height="200" alt="image" src="https://github.com/user-attachments/assets/b9c7998a-2fc2-4ede-8e38-fe0140b53b03" />
<img width="784" height="715" alt="image" src="https://github.com/user-attachments/assets/3d5d6983-79d3-4d31-9f1d-7b807f4d8409" />

##### step 2 (make our balance equal to NaN):
As was supposed on this stage we easily can checkout, because comparing balance and total_price is done in a wrong way:
<img width="1563" height="433" alt="image" src="https://github.com/user-attachments/assets/fd6f5bba-4b0c-4659-acfb-4710d11ce72c" />

##### step 3 (get flag):
<img width="1601" height="421" alt="image" src="https://github.com/user-attachments/assets/39a9814d-61e2-41f7-9199-808bb51eb67d" />

##### Congrats!!



