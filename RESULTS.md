# K6 lab results

## Part 0

- Step 0.1, http_req_duration: avg=281.31ms min=281.31ms med=281.31ms max=281.31ms p(90)=281.31ms p(95)=281.31ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 75 requests, 3.71 req/s, p(95) = 318.82 ms. Each iteration takes about 1.34s, so 5 VUs give about 5 / 1.34 ≈ 3.7 req/s.
- Step 0.3: iterations = 20, run ended after the 20th iteration
- Step 0.6, exit code of the normal run and of the run with P95=300: 0 and 99
- Step 0.7: All 3 rules passed: 'p(95)<1000' p(95)=424.91ms, 'p(99)<1500' p(99)=450.01ms, 'rate<0.01' rate=0.00%. After changing the p(95) limit from 1000 ms to 100 ms (`-e P95=100`), the p(95) rule failed and the exit code was 99.
- Step 0.8, what was wrong, and how the checks showed it:
- Step 0.8: The request used lowercase names ('pepperoni', 'knife'), but the API uses 'Pepperoni' and 'Knife', so nothing was excluded. Before, "no pepperoni" failed 10/75 and "no knife needed" failed 25/75; after the fix, all 225 checks passed. Pizzas are random, so one manual request could look correct by luck, but 75 requests under load showed the failures clearly.

## Part A results

| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     | 0.52/s| 436.49ms| 549.81ms| 0.00%| 100.00%|
| A2   | Average load | 3     | 1.33/s| 474ms | 1s  | 0.00%. | 100.00%|
| A3   | Stress       | 5     | 2.12/s| 392ms | 950ms| 0.00%. | 100.00%|
| A4   | Spike        | 5     | 1/s   | 386ms | 443ms| 0.00%. | 100.00%|
| A5   | Soak         | 2     | 0.98/s| 409ms | 575ms| 0.00%. | 100.00%|

## Part B

1. results table above
2. Each iteration takes about 1.34 s, so 5 VUs give about 5 / 1.34 ≈ 3.7 req/s.
3. No. p(95) from 474 ms (A2) to 392 ms (A3), even though the request rate rose from 1.33 to 2.12 req/s. 5 users is far inside QuickPizza's operational envelope, so the difference is network noise, not load. 
4. p(95) rose from about 360 ms to about 420 ms during the spike, and returned to normal about 10–15 s after the load dropped back to 1 user.
5. No. p(95) stayed around 350–420 ms from start to end, and failed requests stayed at 0%. A longer soak test could reveal a memory leak.
6. The average hides slow requests. In A2 the average was 336 ms, but p(99) was 918 ms, so some users waited almost 3 times longer.
7. Internet latency from Thailand to the server changes over time, while a lab network is short and stable, Other people use QuickPizza at the same time.
8. It works like a denial-of-service attack: it can slow down or crash a system for its real users, and testing without permission may be illegal.
9. Vague: The food delivery app should load fast." Testable: "With 200 users for 10 minutes, the restaurant list API answers 95% of requests within 800 ms and 99% within 1.5 s, and fewer than 0.5% of requests fail.
   thresholds: {
     http_req_duration: ['p(95)<800', 'p(99)<1500'],
     http_req_failed: ['rate<0.005'],
   }