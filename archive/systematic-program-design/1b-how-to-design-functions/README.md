# HtDF 레시피로 배우는 이미지 처리 함수 설계하기

## 이 글에서 얻을 수 있는 가치

- **구조화된 사고**: How to Design Functions(HtDF) 레시피를 따라 함수 설계를 체계적으로 익힐 수 있어요.
- **에러 감소**: 테스트 중심 설계를 통해 예상치 못한 버그를 미연에 방지할 수 있답니다.
- **유연한 적용**: 간단한 예제뿐 아니라 실제 이미지 처리에도 레시피를 유연하게 적용하는 방법을 배워요.

이 글을 읽고 나면, DrRacket이나 Racket 환경이 익숙하지 않아도 HtDF 레시피를 활용해 **이미지의 면적 계산**과 **“키가 큰 이미지” 판별** 함수를 스스로 설계·구현할 수 있게 됩니다.

## 목차

1. [HtDF 레시피란?](#htdf-레시피란)
2. [예시 1: ](#예시-1-image-area-함수-설계)[`image-area`](#예시-1-image-area-함수-설계)[ 함수 설계](#예시-1-image-area-함수-설계)
3. [예시 2: ](#예시-2-tall-함수-설계)[`tall?`](#예시-2-tall-함수-설계)[ 함수 설계](#예시-2-tall-함수-설계)
4. [테스트와 유연성](#테스트와-유연성)
5. [마무리](#마무리)

---

## HtDF 레시피란?

HtDF(How to Design Functions)는 복잡해 보이는 문제를 **작은 단계로 쪼개어** 해결하는 ‘함수 설계 요리법’입니다.

1. **시그니처(Signature)**: 입력과 출력을 어떤 자료형으로 받을지 정해요.
2. **목적문(Purpose Statement)**: 이 함수가 **무엇을 하는지** 구체적으로 설명해요.
3. **스텁(Stub)**: 우선 더미 값을 반환하는 기본 틀을 만듭니다.
4. **테스트(Examples)**: 다양한 경우를 생각해보고 `check-expect`로 작성해요.
5. **템플릿(Template)**: 입력값을 다루는 뼈대를 복사해 두고, 실제 구현에서 재활용합니다.
6. **본체(Body) 구현**: 테스트와 시그니처를 참고해 실제 로직을 채워 넣어요.

단순한 함수에서는 다소 번거로울 수 있지만, 복잡한 함수일수록 이 레시피의 **가치**가 더욱 커진답니다.

---

## 예시 1: `image-area` 함수 설계

```scheme
(require 2htdp/image)

;; image-area-starter.rkt

;
; PROBLEM:
;
; DESIGN a function called image-area that consumes an image and produces the
; area of that image. For the area it is sufficient to just multiple the image's
; width by its height.  Follow the HtDF recipe and leave behind commented
; out versions of the stub and template.
;

; Image -> Natural
; produce area(width * height) of the given image

(check-expect (image-area (rectangle 2 3 "solid" "red")) 6)

;(define (image-area img) 0)

;(define (image-area img)
; (...img))

(define (image-area img)
  (* (image-width img) (image-height img)))
```

- **코드 설명**:

  - `(require 2htdp/image)`로 이미지 처리를 위한 라이브러리를 불러옵니다.
  - 시그니처 `Image -> Natural`과 목적문으로 함수의 입력과 출력을 명확히 지정했어요.
  - `check-expect` 테스트는 실제 이미지 객체를 넣어 면적을 계산하는지 확인해줍니다.
  - 스텁과 템플릿을 남겨 설계 과정을 보여주고, 마지막에 본체 구현을 작성했어요.
  - 본체는 `(image-width img)`와 `(image-height img)`를 곱해 면적을 구합니다.

- **가치**: 이미지의 가로·세로 크기를 곱해 면적을 빠르게 구할 수 있어요.

---

## 예시 2: `tall?` 함수 설계

```scheme
(require 2htdp/image)

;; tall-starter.rkt

;
; PROBLEM:
;
; DESIGN a function that consumes an image and determines whether the
; image is tall.
;
; Remember, when we say DESIGN, we mean follow the recipe.
;
; Leave behind commented out versions of the stub and template.
;

;; Image -> Boolean
;; given an image, produces boolean telling whether image is tall or not
;; produces true if the image is tall
;; produces true if the image is tall (height is greater than width)

(check-expect (tall? (rectangle 2 3 "solid" "red")) true)
(check-expect (tall? (rectangle 3 2 "solid" "red")) false)
(check-expect (tall? (rectangle 3 3 "solid" "red")) false)

;(define (tall? img) false)

;(define (tall? img)
; (... img))

;(define (tall? img)
;  (if (> (image-height img) (image-width img))
;      true
;      false))

(define (tall? img)
  (> (image-height img) (image-width img)))
```

- **코드 설명**:

  - `Boolean` 반환 함수에는 `?` 접미사를 붙여 의도를 분명히 했습니다.
  - 경계 상황(높이 == 너비) 테스트를 추가해 논리적 오류를 방지했어요.
  - 본체는 `>` 연산자를 사용해 높이가 너비보다 큰지만 판단합니다.

- **가치**: 세로로 긴 이미지를 자동으로 필터링하거나 레이아웃에 맞춰 처리할 수 있어요.

---

## 테스트와 유연성

- **충분한 테스트 작성**: 단순히 한 가지 예시만 통과했다고 만족하면 위험해요. 높거나, 낮거나, 같은 경우까지 모두 커버해야 합니다.
- **테스트 확인 순서**: 테스트가 실패하면 **먼저 테스트 코드**를 의심하세요. 테스트 오류를 함수 본체에 반영하면 그게 더 큰 버그가 될 수 있습니다.
- **유연한 설계 흐름**: 시그니처 → 테스트 → 구현 순서를 꼭 지키지 않아도 됩니다. 필요에 따라 앞뒤로 왔다 갔다 하며 더 좋은 설계를 찾아보세요.

> 양념을 먼저 맛보고, 재료를 추가하다 보면 가끔 레시피 순서를 바꿔야 할 때가 있죠. 최종적으로 맛있게 완성되는 게 중요하니까요.

---

## 마무리

HtDF 레시피를 이미지 처리 함수에 적용해 보니, 갑자기 복잡해 보이던 문제도 **한 걸음씩 해체**해 나가며 쉽게 풀 수 있었습니다.

- **구조적 사고**를 통해 설계 과정이 명확해지고,
- **테스트 중심**으로 안정적인 코드를 작성하며,
- **유연한 순서**로 나만의 방식으로 레시피를 응용할 수 있죠.

다음 번에는 더 복잡한 이미지 필터나 변환 함수를 HtDF로 설계해 보는 건 어떨까요? 처음에는 조금 번거롭겠지만, 곧 습관이 되어 **어떤 문제든 차분히 해결**할 힘을 얻게 될 거예요.
행복한 설계 되세요! 😊
