# UIX Assembly
## Instruction Set
### Summary
#### General
Mneumonic | Name
--------- | ----
`COBJ` | ConstructObject
`COBJI` | ConstructObjectIndirect
`COBP` | ConstructObjectParam
`CSTR` | ConstructFromString
`CBIN` | ConstructFromBinary
`INIT` | InitializeInstance
`INITI` | InitializeInstanceIndirect
`LSYM` | LookupSymbol
`WSYM` | WriteSymbol
`WSYMP` | WriteSymbolPeek
`CSYM` | ClearSymbol
`PINI` | PropertyInitialize
`PINII` | PropertyInitializeIndirect
`PLAD` | PropertyListAdd
`PDAD` | PropertyDictionaryAdd
`PASS` | PropertyAssign
`PASST` | PropertyAssignStatic
`PGET` | PropertyGet
`PGETP` | PropertyGetPeek
`PGETT` | PropertyGetStatic
`MINV` | MethodInvoke
`MINVP` | MethodInvokePeek
`MINVT` | MethodInvokeStatic
`MINVA` | MethodInvokePushLastParam
`MINVAT` | MethodInvokeStaticPushLastParam
`VTC` | VerifyTypeCast
`CON` | ConvertType
`OPR` | Operation
`ISC` | IsCheck
`ASC` | As
`TYP` | TypeOf
`PSHN` | PushNull
`PSHC` | PushConstant
`PSHT` | PushThis
`DIS` | DiscardValue
`RET` | ReturnValue
`RETV` | ReturnVoid
`JMPF` | JumpIfFalse
`JMPFP` | JumpIfFalsePeek
`JMPTP` | JumpIfTruePeek
`JMPD` | JumpIfDictionaryContains
`JMPNP` | JumpIfNullPeek
`JMP` | Jump
`CLIS` | ConstructListenerStorage
`LIS` | Listen
`LISD` | DestructiveListen
`DBG` | EnterDebugState

#### Math Operators
Mneumonic | Name
--------- | ----
`ADD` | MathAdd
`SUB` | MathSubtract
`MUL` | MathMultiply
`DIV` | MathDivide
`MOD` | MathModulus
`NEG` | MathModulus
`AND` | LogicalAnd
`ORR` | LogicalOr
`NOT` | LogicalNot
`REQ` | RelationalEquals
`RNE` | RelationalNotEquals
`RLT` | RelationalLessThan
`RGT` | RelationalGreaterThan
`RLE` | RelationalLessThanEquals
`RGE` | RelationalGreaterThanEquals
`RIS` | RelationalIs
`INC` | PostIncrement
`DEC` | PostDecrement


## TODO

### `IStringEncodable`
> So I added an interface named `IStringEncodable` that has what's essentially a specialized `ToString()` method. Each schema has a matching runtime type, and if the schema supports converting from a string, I modify the runtime class to implement `IStringEncodable` and returns a valid string that UIX already knows how to parse.

> It's a pretty manual process, so I've been implementing that interface as I observe incorrect constants in the disassembly. Turns out the `Image` schema supports string encoding, but `UIImage` (the corresponding runtime class) didn't implement `IStringEncodable` so it has the wrong representation in the disassembly.
