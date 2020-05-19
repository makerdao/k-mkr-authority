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

# Rule for setRoot proof (magic nubmer is bitwise not of 2^160 - 1).
```k
rule 115792089237316195423570985007226406215939081747436879206741300988257197096960 &Int X => 0
  requires #rangeAddress(X)
```

# Lemmas for bytes4 support
```k
syntax TypedArg ::= #bytes4( Int )

syntax String ::= #typeName ( TypedArg ) [function]
rule #typeName(    #bytes4( _ )) => "bytes4"

syntax Int ::= #lenOfHead ( TypedArg ) [function]
rule #lenOfHead(#bytes4( _ )) => 32

syntax Bool ::= #isStaticType ( TypedArg ) [function]
rule #isStaticType(   #bytes4( _ )) => true

syntax WordStack ::= #enc ( TypedArg ) [function]
rule #enc( #bytes4( DATA )) => #buf(32, #getValue( #bytes4( DATA )))

syntax Int ::= "maxBytes4"
  rule maxBytes4 => 115792089210356248756420345214020892766250353992003419616917011526809519390720 [macro] /* (2^32 - 1) * 2^224 */

syntax Int ::= #getValue ( TypedArg ) [function]
rule #getValue(#bytes4( DATA )) => DATA
  requires minUInt256 <=Int DATA andBool DATA <=Int maxBytes4

rule 115792089210356248756420345214020892766250353992003419616917011526809519390720 &Int X => X
  requires minUInt256 <=Int X andBool X <=Int maxBytes4

rule X &Int 115792089210356248756420345214020892766250353992003419616917011526809519390720 => X
  requires minUInt256 <=Int X andBool X <=Int maxBytes4
```
