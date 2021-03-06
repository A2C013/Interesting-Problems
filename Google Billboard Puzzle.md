#An inspiration of MATLAB's calculation precision to solve the [Google Billboar Puzzle](http://google-tale.blogspot.jp/2008/07/google-billboard-puzzle.html).

Recently in a documentary _**The Internet Age**_ produced by CNTV, I read about the story about **Google's math puzzle-- the first 10-digit prime found in consecutive digits of _e_**. Then I tried to figure it out, but soon I found that I was trapped in solving the calculation precision problem. No matter how I tried to improve the methods to approximate the natural logarithm e, without exception, I failed to confirm the computation accuracy, which means that I am not sure whether the n-digits number I got is right the same as _**e**_ . 

Then I turn to the website for help and soon I found a blog named [Googol: Google Billboard Puzzle](http://google-tale.blogspot.jp/2008/07/google-billboard-puzzle.html), here is a brief introduction about the history of the puzzle:
> #### Google Billboard Puzzle
> In July 2004, Google tried a innovative way of recruiting talented people, by posting a mathematical puzzle on a billboard on 'Highway 101' in the heart of silicon valley. The billboard read:

> > **{first 10-digit prime found in consecutive digits of e}.com.**

The author of the blog also tried to solve the problem though it has been already 4 years later. His calculated Euler's number 'e' to a bit more than 10000 digits with the help of Java's "BigDecimal" and then the puzzle was easily solved. 

Then I tried to transplant the algorithm to **Python** and found that Python had a similar module "Decimal", which can complete the whole computation with extraordinary precision, so the work will be a bit "worthless" and under-challenge.

To make it more interesting and challenging, I decided to solve the problem with MATLAB. Soon cames out the same problem, the default output format for float variables in MATLAB is **SHORT**, I can only see several digits behind the decimal point. Though MATLAB has a function **format** to change the display form, unfortunately, it does not affect the way how MATLAB calculations are done, which means **format** has nothing to do with the calculation precision.

But **digits** and **vpa** can help to solve the accuracy problem.
* **digits** set variable precision digits, the default value is *32* digits.
Syntax:
`digits(D) % D determines the accuracy of variable precision numeric computations.`

* **vpa**(variable precision arithmetic.)

Syntax:
```Matlab
R = vpa(A, d) % use at least d significant (nonzero) digits, instead of the current setting of digits.
              % A is a symbolic object, string, or numeric expression 
              % d is an integer greater than 1 and smaller than 2^29 + 1
```

> It is important to avoid the evaluation of an expression using double precision floating point arithmetic before it is passed to vpa.
>     For example,
>        `phi = vpa((1+sqrt(5))/2)`
>     first computes a 16-digit approximation to the golden ratio, then converts that approximation to one with d digits, where d is the current setting of DIGITS. To get full precision, use unevaluated string or symbolic arguments,
>        `phi = vpa('(1+sqrt(5))/2')`
>     or
>        `s = sym('sqrt(5)')`
>        `phi = vpa((1+s)/2);`

With all the info we get, the puzzle can be solved quickly using the codes below: 
```Matlab
EulersNum = char(vpa('exp(1)',200));
index = find(EulersNum == '.');
EulersNum(index) = [];
n = length(EulersNum);
for i=1:n-9
    if isprime(str2num(EulersNum(i:i+9)))
        fprintf('first 10-digit prime: %d\n',str2num(EulersNum(i:i+9)))
        break
    end
end
```
Certainly, the answer is `first 10-digit prime: **7427466391.**`

Finally, I have to confess that the algorithm here is a bit opportunist, firstly I don't calculate the Euler's number(just use **vpa** to get 200 digits of e), and then I use a built-in function **isprime** to judge whether an integer n is a prime or not. Obviously, we can define a function to realize the same function.

```Matlab
function tf = isprime(n)
    tf = 1;
    for i = 1:floor(sqrt(n))+1
        if mod(n, i) == 0
            tf = 0;
            break;
    end
end
```
