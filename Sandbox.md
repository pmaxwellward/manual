# Header 1
## Header 2
### Header 3
#### Header 4
##### Header 5
###### Header 6

Some `inline code() {}`.

```cpp
double getTime() {
	struct timespec ts;
	clock_gettime(CLOCK_MONOTONIC_RAW, &ts);
	int64_t time = int64_t(ts.tv_sec) * 1000000000LL + ts.tv_nsec;
	return (time - startTime) / 1e9;
}
```

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

This is $\int x dx = \frac{1}{2} x^2 + C = x^2 / 2 + C$ inline math.

$$f(k) = \sum{i=1}^\infty \frac{1}{n^k}$$
