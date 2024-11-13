# UIX Assembly

## TODO
> So I added an interface named `IStringEncodable` that has what's essentially a specialized `ToString()` method. Each schema has a matching runtime type, and if the schema supports converting from a string, I modify the runtime class to implement `IStringEncodable` and returns a valid string that UIX already knows how to parse.

> It's a pretty manual process, so I've been implementing that interface as I observe incorrect constants in the disassembly. Turns out the `Image` schema supports string encoding, but `UIImage` (the corresponding runtime class) didn't implement `IStringEncodable` so it has the wrong representation in the disassembly.

> UIX has three different ways of "persisting" constants: string, binary, and canonical. Canonical here is basically like accessing a static property on a control type using the property name. Previously, I treated canonical constants the same as string constants, but I have a theory that the persist mode matters more than I thought. I suspect that's why I'm getting "index out of bounds" errors when trying to recompile UIXA files, because somewhere the markup system mistook a canonical mode as string mode and is looking in the wrong place.