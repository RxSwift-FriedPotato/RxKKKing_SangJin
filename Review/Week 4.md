# ๐Traits๋?

RxSwift๋ Observable์ ์ฌ์ฉํ  ๋ ๋ชํํ ์ด๋ฒคํธ ๋ฐ์ ๊ท์น์ ๊ฐ์ง ์ ์๋๋ก Traits๋ฅผ ์ง์ํฉ๋๋ค.

Traits๋ RxSwift๋ฅผ ์ฌ์ฉํ  ๋ ์ฝ๋๋ฅผ ๋ชํํ๊ณ  ์ง๊ด์ฑ ์๊ฒ ๊ฐ์ง๊ณ ์ ํ  ๋ ์ ํ์ ์ผ๋ก ์ฌ์ฉํ  ์ ์์ผ๋ฉฐ, ์ฝ๋์ ์๋๋ฅผ ํ์คํ ๋ณด์ฌ์ค ์ ์๋ค๋ ์ฅ์ ์ด ์์ต๋๋ค.

RxSwift์ Traits๋ ๋ค์๊ณผ ๊ฐ์ 3๊ฐ์ง ์ข๋ฅ๋ฅผ ๊ฐ๊ณ  ์์ต๋๋ค.

1. Single : ํญ์ ๋จ์ผ ์์ ๋๋ ์๋ฌ๋ฅผ ๋ฐฉ์ถํ๋๋ก ๋ณด์ฅํ๋ ์ํ์ค
2. Completable : ์ด๋ฒคํธ์ ๋ํด ์๋ฃ ๋๋ ์๋ฌ๋ฅผ ๋ฐฉ์ถํ๊ณ , ์์๋ ์๋ฌด๊ฒ๋ ๋ฐฉ์ถํ์ง ์๋ ๊ฒ์ ๋ณด์ฅํ๋ ์ํ์ค
3. Maybe : Single๊ณผ Completable์ ์ค๊ฐ ํน์ฑ์ ๊ฐ์ง๋ ์ํ์ค

<br>

## ๐Single

Single์ Observable์ด ๋ณํ๋ ๊ฒ์ผ๋ก, **ํญ์ ๋จ์ผ ์์** ๋๋ **์ค๋ฅ**๋ง์ ๋ฐฉ์ถํ๊ธฐ ๋๋ฌธ์ ์ฃผ๋ก HTTP ์์ฒญ์ ์ฒ๋ฆฌํ๋๋ฐ ์ฌ์ฉ๋ฉ๋๋ค.

Single์ Observable๊ณผ ๋ค๋ฅด๊ฒ onSuccess, onFailure๋ง ์ฒ๋ฆฌํ๋ฉด ๋ฉ๋๋ค.<br>๋ํ Single์ ๊ธฐ๋ณธ์ ์ผ๋ก Observable์ด๊ธฐ ๋๋ฌธ์, ๊ธฐํ Observable ์ฐ์ฐ์๋ค์ ์ฌ์ฉํ  ์ ์์ต๋๋ค.

**์ฝ๋ ์์**

```
func getRequest(url: String?) -> Single<Bool> {
    return Single<Bool>.create { single in
        guard let url = url else {
            single(.failure(NSError.init(domain: "error", code: -1, userInfo: nil)))
            return Disposables.create()
        }

        if url == "https://www.google.com" {
            single(.success(true))
        } else {
            single(.success(false))
        }

        return Disposables.create()
    }
}

getRequest(url: "https://www.github.com")
    .subscribe(onSuccess: { success in
        print("url ํ๋ณ: ", success)
    }, onFailure: { error in
        print("Error: ", error)
    })
    .dispose()



**์ถ๋ ฅ ๊ฒฐ๊ณผ**
url ํ๋ณ: false
```

<br>

## ๐Completable

Completable์ completed, error ๋ ๊ฐ์ง ์ด๋ฒคํธ๋ฅผ ๋ฐฉ์ถํ  ์ ์์ผ๋ฉฐ, ์๋ฌด ์์๋ ๋ฐฉ์ถํ์ง ์๊ธฐ ๋๋ฌธ์ ๋จ์ ์๋ฃ ์ฌ๋ถ๋ง ์๊ณ  ์ถ์ ๋ ์ฌ์ฉํฉ๋๋ค. (์๋ฃ์ ๋ฐ๋ฅธ ์์์ ๋ํด ์ ๊ฒฝ์ฐ์ง ์์๋ ๋๋ ๊ฒฝ์ฐ)

**์ฝ๋ ์์**

```
func completable() -> Completable {
    return Completable.create { completable in 
    // Store some data locally
    
        guard success else { 
            completable(.error(CacheError.failedCaching)) 
            return Disposables.create {} 
            } 
        completable(.completed)     
        return Disposables.create {} 
    } 
} 

completable().subscribe(
    onCompleted: { print("Completed with no error") 
    }, 
    
    onError: { error in print("Completed with an error: \(error.localizedDescription)") 
    } 
).disposed(by: disposeBag)

//์ฌ์ฉ๋ฒ
getRepo("ReactiveX/RxSwift")
    .subscribe { event in
        switch event {
            case .success(let json):
                print("JSON: ", json)
            case .error(let error):
                print("Error: ", error)
        }
    }
    .disposed(by: disposeBag)
```

## ๐Maybe

Maybe๋ Success, completed, error๋ก 3๊ฐ์ง ์ด๋ฒคํธ๋ฅผ ๋ฐฉ์ถํ  ์ ์์ต๋๋ค.<br>๋ํ Maybe๋ Single๊ณผ Completable ์ฌ์ด์ ์๋ Observable์ ๋ณํ์ผ๋ก, ๋จ์ผ ์์๋ฅผ ๋ฐฉ์ถํ๊ฑฐ๋ ์์๋ฅผ ๋ฐฉ์ถํ์ง ์๊ณ  ์๋ฃํ  ์ ์์ต๋๋ค.

**์ฝ๋ ์์**
```
func maybe() -> Maybe<String> {
    return Maybe<String>.create { maybe in 
        maybe(.success("RxSwift")) 
        // or
        maybe(.completed) 
        // or 
        maybe(.error(error)) 
        
        return Disposables.create {} 
        } 
} 

maybe().subscribe( 
    onSuccess: { 
        element in print("Completed with element \(element)") 
    }, 
    onError: { error in print("Completed with an error \(error.localizedDescription)") 
    }, 
    onCompleted: { print("Completed with no element") 
} ).disposed(by: disposeBag
```