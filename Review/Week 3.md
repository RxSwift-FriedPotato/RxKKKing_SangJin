# ๐Operators๋?

- RxSwift์์ Operator(์ฐ์ฐ์)๋ Observable์ ๋ค๋ฃจ๋ ๋ฉ์๋๋ค์ ํต์นญํ๋ ์ฉ์ด์๋๋ค.
- ์ฝ๊ฒ Observable์ ์์ฑํ๊ณ , ๋ณํํ๊ณ , ํฉ์น๋ ๋ฑ ๋ค์ํ ์ฐ์ฐ์ ํ  ์ ์๋๋ก ๋์์ฃผ๋ ์ญํ ์ ํฉ๋๋ค.
- [Operators์ ์ข๋ฅ](http://reactivex.io/documentation/ko/operators.html)๋ ์๋นํ ๋ง์์, ์ฃผ๋ก ๋ง์ด ์ฌ์ฉ๋๋ 4๊ฐ์ง๋ฅผ ๋ฝ์๋ณด๋ฉด ๋ค์๊ณผ ๊ฐ์ต๋๋ค.

<br>

# ๐์ฃผ์ Operators

### ๐just

- ํ๋์ Element๋ฅผ Observable๋ก ๋ง๋๋ ์ญํ ์ ํ๋ค.

**์ฝ๋ ์์**

<pre>
<code>
        func exJust1() {
        Observable.just("Hello World")
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
            }
</code>
</pre>

**์ถ๋ ฅ ๊ฒฐ๊ณผ**

<pre>
<code>
Hello World
</code>
</pre>

<br>

### ๐from

- array, dictionary, set ๋ฑ์ ๋ฐฐ์ด ํํ๋ฅผ Observable Sequence๋ก ๋ง๋๋ ์ญํ ์ ํ๋ค.

**์ฝ๋ ์์**

<pre>
<code>
        func exFrom1() {
        Observable.from(["RxSwift", "In", "4", "Hours"])
            .subscribe(onNext: { str in
                print(str)
            })
            .disposed(by: disposeBag)
            }
</code>
</pre>

**์ถ๋ ฅ ๊ฒฐ๊ณผ**

<pre>
<code>
RxSwift
In
4
Hours
</code>
</pre>

<br>

### ๐map

- ์์์ ๋ด๋ ค์จ ๋ฐ์ดํฐ๋ฅผ ๊ฐ๊ณตํ๋ ์ญํ ์ ํ๋ค.

**์ฝ๋ ์์**

<pre>
<code>
        func exMap1() {
            print("\nexMap1()")
            Observable.just("Hello")
                .map { str in "\(str) RxSwift" }
                .subscribe(onNext: { str in
                    print(str)
                })
                .disposed(by: disposeBag)
        }
</code>
</pre>

**์ถ๋ ฅ ๊ฒฐ๊ณผ**

<pre>
<code>
exMap1()
Hello RxSwift
</code>
</pre>

<br>

### ๐filter

- ์์์ ๋ด๋ ค์จ ๋ฐ์ดํฐ๋ฅผ ์ ๋ณํ๋ ์ญํ ์ ํ๋ค.

**์ฝ๋ ์์**

<pre>
<code>
        func exFilter() {
            print("\nexFilter()")
            Observable.from([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
                .filter { $0 % 2 == 0 }
                .subscribe(onNext: { n in
                    print(n)
                })
                .disposed(by: disposeBag)
        }
</code>
</pre>

**์ถ๋ ฅ ๊ฒฐ๊ณผ**

<pre>
<code>
exFilter()
2
4
6
8
10
</code>
</pre>

Operators๋ ๊ฐ๋ ์์ฒด๋ ์ด๋ ค์ด๊ฒ ์์ง๋ง, ์ํฉ์ ๋ฐ๋ผ ์ด๋ค Operator๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ด ์ ์ ํ์ง ํ๋จํ๋ ๊ฒ๊ณผ,<br>
Operator ์ข๋ฅ ์์ฒด๊ฐ ๋ง๊ธฐ ๋๋ฌธ์ ์์ฃผ ์ฌ์ฉ๋๋ Operator๋ฅผ ์์๋๋ ๊ฒ์ด ์ค์ํ ๊ฒ ๊ฐ์ต๋๋ค.