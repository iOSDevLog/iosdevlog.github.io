---
layout: post
title: "Filtering Operators"
description: "RxSwift"
category: RxSwift
tags: [RxSwift, iOS]
---

# RxMarbles for iOS

![RxMarbles](https://raw.githubusercontent.com/RxSwiftCommunity/RxMarbles/master/rxmarbles.jpg)

# This Could Be Us But You Playing

<https://github.com/neonichu/ThisCouldBeUsButYouPlaying>

```bash
$ gem install cocoapods-playgrounds
```

```swift
public func example(of description: String, action: () -> Void) {
  print("\n--- Example of:", description, "---")
  action()
}
```

## Ignoring operators
---

### ignoreElements

![ignoreElements](/assets/images/Rx/RxSwift/filtering-operators/ignoreElements.png)

```swift
//: Please build the scheme 'RxSwiftPlayground' first
import RxSwift

example(of: "ignoreElements") {

  // 1
  let strikes = PublishSubject<String>()

  let disposeBag = DisposeBag()

  // 2
  strikes
    .ignoreElements()
    .subscribe { _ in
      print("You're out!")
    }
    .addDisposableTo(disposeBag)

  strikes.onNext("X")
  strikes.onNext("X")
  strikes.onNext("X")
  strikes.onCompleted()
}
```

> --- Example of: ignoreElements ---
>
> You're out!

### elementAt

![elementAt](/assets/images/Rx/RxSwift/filtering-operators/elementAt.png)

```swift
example(of: "elementAt") {

  // 1
  let strikes = PublishSubject<String>()

  let disposeBag = DisposeBag()

  //  2
  strikes
    .elementAt(2)
    .subscribe(onNext: { _ in
      print("You're out!")
    })
    .addDisposableTo(disposeBag)

  strikes.onNext("X")
  strikes.onNext("X")
  strikes.onNext("X")

}
```

> --- Example of: elementAt ---
>
> You're out!

### filter

![filter](/assets/images/Rx/RxSwift/filtering-operators/filter.png)


```swift
example(of: "filter") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of(1, 2, 3, 4, 5, 6)
    // 2
    .filter { integer in
      integer % 2 == 0
    }
    // 3
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)

}
```

> --- Example of: filter ---
>
> 2
>
> 4
>
> 6

## Skipping operators
---

### skip

![skip](/assets/images/Rx/RxSwift/filtering-operators/skip.png)


```swift
example(of: "skip") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of("A", "B", "C", "D", "E", "F")
    // 2
    .skip(3)
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: skip ---
>
> D
>
> E
>
> F

### skipWhile

![skipWhile](/assets/images/Rx/RxSwift/filtering-operators/skipWhile.png)


```swift
example(of: "skipWhile") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of(2, 2, 3, 4, 4)
    // 2
    .skipWhile { integer in
      integer % 2 == 0
    }
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: skipWhile ---
>
> 3
>
> 4
>
> 4

### skipUntil

![skipUntil](/assets/images/Rx/RxSwift/filtering-operators/skipUntil.png)

```swift
example(of: "skipUntil") {

  let disposeBag = DisposeBag()

  // 1
  let subject = PublishSubject<String>()
  let trigger = PublishSubject<String>()

  // 2
  subject
    .skipUntil(trigger)
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)

  subject.onNext("A")
  subject.onNext("B")

  trigger.onNext("X")
  subject.onNext("C")
}
```

> --- Example of: skipUntil ---
>
> C

## Taking operators
---

### take

![take](/assets/images/Rx/RxSwift/filtering-operators/take.png)

```swift
example(of: "take") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of(1, 2, 3, 4, 5, 6)
    // 2
    .take(3)
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: take ---
>
> 1
>
> 2
>
> 3

### takeWhileWithIndex

![takeWhileWithIndex](/assets/images/Rx/RxSwift/filtering-operators/takeWhileWithIndex.png)

```swift
example(of: "takeWhileWithIndex") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of(2, 2, 4, 4, 6, 6)
    // 2
    .takeWhileWithIndex { integer, index in
      // 3
      integer % 2 == 0 && index < 3
    }
    // 4
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: takeWhileWithIndex ---
>
> 2
>
> 2
>
> 4

### takeUntil

![takeUntil](/assets/images/Rx/RxSwift/filtering-operators/takeUntil.png)

```swift
example(of: "takeUntil") {

  let disposeBag = DisposeBag()

  // 1
  let subject = PublishSubject<String>()
  let trigger = PublishSubject<String>()

  // 2
  subject
    .takeUntil(trigger)
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)

  // 3
  subject.onNext("1")
  subject.onNext("2")

  trigger.onNext("X")

  subject.onNext("3")
}
```

> --- Example of: takeUntil ---
>
> 1
>
> 2

## Distinct operators
---

### distinctUntilChanged

![distinctUntilChanged](/assets/images/Rx/RxSwift/filtering-operators/distinctUntilChanged.png)


```swift
example(of: "distinctUntilChanged") {

  let disposeBag = DisposeBag()

  // 1
  Observable.of("A", "A", "B", "B", "A")
    // 2
    .distinctUntilChanged()
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: distinctUntilChanged ---
>
> A
>
> B
>
> A

### distinctUntilChanged_more

![distinctUntilChanged_more](/assets/images/Rx/RxSwift/filtering-operators/distinctUntilChanged_more.png)

```swift
example(of: "distinctUntilChanged(_:)") {

  let disposeBag = DisposeBag()

  // 1
  let formatter = NumberFormatter()
  formatter.numberStyle = .spellOut

  // 2
  Observable<NSNumber>.of(10, 110, 20, 200, 210, 310)
    // 3
    .distinctUntilChanged { a, b in
      // 4
      guard let aWords = formatter.string(from: a)?.components(separatedBy: " "),
        let bWords = formatter.string(from: b)?.components(separatedBy: " ")
        else {
          return false
      }

      var containsMatch = false

      // 5
      for aWord in aWords {
        for bWord in bWords {
          if aWord == bWord {
            containsMatch = true
            break
          }
        }
      }

      return containsMatch
    }
    // 4
    .subscribe(onNext: {
      print($0)
    })
    .addDisposableTo(disposeBag)
}
```

> --- Example of: distinctUntilChanged(_:) ---
> 10
> 20
> 200