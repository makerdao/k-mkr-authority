Copyright (C) 2019 Maker Ecosystem Growth Holdings, INC.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

```smt2


(set-option :auto-config false)
(set-option :smt.mbqi false)
;(set-option :smt.mbqi.max_iterations 15)

; (set-option :auto-config false)
; (set-option :smt.mbqi false)
; (set-option :smt.array.extensional false)

; int extra
(define-fun int_max ((x Int) (y Int)) Int (ite (< x y) y x))
(define-fun int_min ((x Int) (y Int)) Int (ite (< x y) x y))
(define-fun int_abs ((x Int)) Int (ite (< x 0) (- 0 x) x))

; bool to int
(define-fun smt_bool2int ((b Bool)) Int (ite b 1 0))

; ceil32
(define-fun ceil32 ((x Int)) Int ( * ( div ( + x 31 ) 32 ) 32 ) )

;; pow256 and pow255
(define-fun pow256 () Int
  115792089237316195423570985008687907853269984665640564039457584007913129639936)
(define-fun pow255 () Int
  57896044618658097711785492504343953926634992332820282019728792003956564819968)
;; weird declaration trick (doesn't seem to be needed currently)
;; (declare-fun pow256 () Int)
;; (assert (>= pow256 115792089237316195423570985008687907853269984665640564039457584007913129639936))
;; (assert (<= pow256 115792089237316195423570985008687907853269984665640564039457584007913129639936))

;; signed word arithmetic definitions:
;; integer to word:
(define-fun unsigned ((x Int)) Int
  (if (>= x 0)
      x
    (+ x pow256)))

;; word to integer
(define-fun signed ((x Int)) Int
  (if (< x pow255)
      x
    (- x pow256)))

;; signed_abs
(define-fun signed_abs ((x Int)) Int
  (if (< x pow255)
      x
    (- pow256 x)))

;; signed_sgn
(define-fun signed_sgn ((x Int)) Int
  (if (< x pow255)
      1
    -1))

;
; end of prelude
;
```
