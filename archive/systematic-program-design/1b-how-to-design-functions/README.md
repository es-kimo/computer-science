# HtDF 레시피로 배우는 함수 설계하기

다음과 같은 문제가 있습니다.

```scheme
PROBLEM:

Design a function that consumes two images and produces true if the first is larger than the second.
```

당신이라면 이 함수를 어떻게 구현하실 건가요?

함수 설계가 막막할 때 참고하기 좋은 것을 소개해볼게요.

HtDF 레시피를 따라가다보면 문제가 훨씬 간단해질 겁니다.

## HtDF 레시피란?

### HtDF 레시피로 요리한 함수 예시

## 문제 해결

다시 문제로 돌아와볼게요.

1. Signature

```scheme
;; Image Image -> Boolean
```

2. Purpose

```scheme
;; produces true if the first image's area is larger than the second's
```

- Purpose 문을 작성하다보니 한 가지 의문이 들었어요.
- 'larger'에 대한 정의가 필요하겠다.
- 면적을 비교해서 어떤 게 큰지 결정해볼 거예요.
- 그런데 이미지는 사각형만 있지 않고, 원, 사다리꼴 등등 다양해요.
- 이미지가 어떤 형태이냐에 따라 면적을 구하는 방식이 달라지기 때문에 body를 작성하기 까다로울 것으로 보여요.
- 따라서 1번 Signature로 돌아가 매개변수를 Image가 아닌 Rectangle로 구체화 시킬 거예요.
- 그러면 Purpose도 다음과 같이 수정할 수 있어요.

```scheme
;; produces true if the first rectangle's area is larger than the second's
```

3. Test

```scheme
(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 7 3 "solid" "red")) false)
(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 5 3 "solid" "red")) true)
(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 5 4 "solid" "red")) false)
```

- 가능한 경우의 수는 총 3개이므로 테스트도 3개를 작성해주었어요.

4. Stub

```scheme
; (define (compare-area? rect1 rect2) true)
```

5. Template

```scheme
;(define (compare-area? rect1 rect2
 ;                      (... rect1 rect2)))
```

6. Body

```scheme
(define (compare-area? rect1 rect2)
                       (> (* (image-width rect1) (image-height rect1))
                          (* (image-width rect2) (image-height rect2))))
```

7. Full Code

```scheme
(require 2htdp/image)

;; Rectangle, Rectangle -> Boolean
;; produces true if the first rectangle's area is larger than the second's

(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 7 3 "solid" "red")) false)
(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 5 3 "solid" "red")) true)
(check-expect (compare-area? (rectangle 10 2 "solid" "red") (rectangle 5 4 "solid" "red")) false)


; (define (compare-area? rect1 rect2) true)

;(define (compare-area? rect1 rect2
 ;                      (... rect1 rect2)))

(define (compare-area? rect1 rect2)
                       (> (* (image-width rect1) (image-height rect1))
                          (* (image-width rect2) (image-height rect2))))
```
