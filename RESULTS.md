# K6 lab results

## Part 0

- Step 0.1, http_req_duration: avg=281.31ms min=281.31ms med=281.31ms max=281.31ms p(90)=281.31ms p(95)=281.31ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 75 requests, 3.71 req/s, p(95) = 318.82 ms. Each iteration takes about 1.34s, so 5 VUs give about 5 / 1.34 ≈ 3.7 req/s.
- Step 0.3: iterations = 20, run ended after the 20th iteration
- Step 0.6, exit code of the normal run and of the run with P95=300: 0 and 99
- Step 0.7, rules that passed, p(95), p(99), exit code after your change:
- Step 0.8, what was wrong, and how the checks showed it:

## Part A results

| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     |       |       |     |        |        |
| A2   | Average load | 3     |       |       |     |        |        |
| A3   | Stress       | 5     |       |       |     |        |        |
| A4   | Spike        | 5     |       |       |     |        |        |
| A5   | Soak         | 2     |       |       |     |        |        |

## Part B

1.
2.
3.
4.
5.
6.
7.
8.
9.