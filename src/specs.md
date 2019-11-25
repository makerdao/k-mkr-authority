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

```act
behaviour wards of MkrAuthority
interface wards(address usr)

types
  May : uint256

storage
  wards[usr] |-> May

iff
  VCallValue == 0

returns May
```

```act
behaviour root of MkrAuthority
interface root()

types
  Root : address

storage
  root |-> Root

iff
  VCallValue == 0

returns Root
```

```act
behaviour setRoot of MkrAuthority
interface setRoot(address usr)

types
  Root : address

storage
  root |-> Root => usr

iff
  VCallValue == 0
  CALLER_ID == Root
```

```act
behaviour rely of MkrAuthority
interface rely(address usr)

types
  Root : address

storage
  root |-> Root
  wards[usr] |-> _ => 1

iff
  VCallValue == 0
  CALLER_ID == Root
```

```act
behaviour deny of MkrAuthority
interface deny(address usr)

types
  Root : address

storage
  root |-> Root
  wards[usr] |-> _ => 0

iff
  VCallValue == 0
  CALLER_ID == Root
```

```act
behaviour canCall-true of MkrAuthority
interface canCall(address src, address dst, bytes4 sig)

types
  Root : address
  May : uint256

storage
  root |-> Root
  wards[src] |-> May

iff
  VCallValue == 0

if
  (src == Root) or (sig == #asWord(#padRightToWidth(32, #take(4, #abiCallData("burn", #address(0), #uint256(0)))))) or (sig == #asWord(#padRightToWidth(32, #take(4, #abiCallData("mint", #address(0), #uint256(0))))) and May == 1)
  sig <= 115792089210356248756420345214020892766250353992003419616917011526809519390720 
  sig >= 0

returns 1
```

```act
behaviour canCall-false of MkrAuthority
interface canCall(address src, address dst, bytes4 sig)

types
  Root : address
  May : uint256

storage
  root |-> Root
  wards[src] |-> May

iff
  VCallValue == 0

if
  src =/= Root
  sig =/= #asWord(#padRightToWidth(32, #take(4, #abiCallData("burn", #address(0), #uint256(0))))) 
  sig =/= #asWord(#padRightToWidth(32, #take(4, #abiCallData("mint", #address(0), #uint256(0))))) or May =/= 1
  sig <= 115792089210356248756420345214020892766250353992003419616917011526809519390720 
  sig >= 0

returns 0
```
