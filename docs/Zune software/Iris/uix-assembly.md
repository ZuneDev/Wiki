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

### Object instantiation
This family of instructions calls the constructor for the given object. All variants take the index of the object type to construct.

#### ConstructObject
Calls the default constructor with no parameters.
```asm
COBJ <typeIndex>
```

#### ConstructObjectIndirect
Used to create 'wrappers' of another constructed object. It is used for type schemas that implement `IDynamicConstructionSchema`, which appears to only be used by `UIClassTypeSchema` for creating the root `<UI>` element. The assignment type is taken from the first operand, while the target type is popped off the stack.
```asm
COBJI <assignmentTypeIndex>
```

#### ConstructObjectParam
Calls the specified constructor with *N* parameters popped from the stack, with the *N*th parameter at the top of the stack. The number of parameters must match the constructor exactly.
```asm
COBP <typeIndex> <constructorIndex>
```

#### ConstructFromString
Uses the target's type converter to construct an instance from a constant string.
```asm
CSTR <typeIndex> <stringIndex>
```

#### ConstructFromBinary
Uses the target type's binary decoder to construct an instance from a binary blob. The blob is stored inline and can be any length. The binary decoder is responsible for determining the length and consuming the entire blob.

#### InitializeInstance
Pops an object from the stack and initializes it using the specified type schema. This instruction is required to initialize `UIClass` and `Class` objects immediately after construction.
```asm
INIT <typeIndex>
```

#### InitializeInstanceIndirect
Identical to [InitializeInstance](#initializeinstance), except the type schema is popped off the stack rather than indexed from the type imports table.
```asm
INITI
```

### Symbols
This family of instructions manages symbols, usually local variables.

#### LookupSymbol
Looks up a symbol in the symbol reference table and pushes its value to the stack.
```asm
LSYM <symbolReferenceIndex>
```

#### WriteSymbol
Writes a value popped from the stack to the specified symbol in the symbol reference table.
```asm
WSYM <symbolReferenceIndex>
```

#### WriteSymbolPeek
Identical to [WriteSymbol](#writesymbol), except the value to be written is peeked instead of poppsed off the stack.
```asm
WSYMP <symbolReferenceIndex>
```

#### ClearSymbol
If the specified symbol is a scoped local, it is undeclared. Otherwise, this instruction is a no-op.
```asm
CSYM <symbolReferenceIndex>
```

#### 

## TODO

### `IStringEncodable`
> So I added an interface named `IStringEncodable` that has what's essentially a specialized `ToString()` method. Each schema has a matching runtime type, and if the schema supports converting from a string, I modify the runtime class to implement `IStringEncodable` and returns a valid string that UIX already knows how to parse.

> It's a pretty manual process, so I've been implementing that interface as I observe incorrect constants in the disassembly. Turns out the `Image` schema supports string encoding, but `UIImage` (the corresponding runtime class) didn't implement `IStringEncodable` so it has the wrong representation in the disassembly.
