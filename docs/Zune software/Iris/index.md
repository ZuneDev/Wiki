# Microsoft Iris

## Subtopics
- [Compiled UIX v4](./compiled-uix.md)
- [Compiled UIX v3](./compiled-uix-3.md)

## What is Microsoft Iris?
Microsoft Iris (sometimes called UIX) is a private UI framework for Windows. It was developed for internal use by various Microsoft Windows software, although only the Zune desktop software is known to use it.

Unlike most modern UI frameworks, Iris does not provide default control styles-- library consumers are expected to define their own styles. However, some later versions of Iris included `UIXControls.dll`, a resource library containing some limited styles for built-in controls. UI layouts are defined in XML-based `.uix` files, which are nearly identical in structure to [Media Center Markup Language (MCML)](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/windows-media-center-sdk/bb189388(v=msdn.10)). Starting with Iris 3, a compiler was included that processed the XML-based layouts into a proprietary binary format.

Parts of the code suggest that Iris may have supported the Xbox 360 (referred to by its codename, Xenon). There is also evidence that a native version of the library was used by some in-box applications in Windows Phone 7, where it was referred to as "Splash".

## What does Microsoft Iris have to do with Zune?
Zune software 2.0 introduced a new UI design, which later become the Metro design language. This was also the first version to utilize the then-new Iris framework. Iris's assembly version generally aligns with the Zune release it came with-- the first known release of Iris is 2.0 for this reason. Iris 2 also did not have the concept of "compiled UIX", so Zune 2.x releases contain readable UIX source code.

The Zune desktop software is partially written in C#, namely `ZuneShell.dll`. This library contains the entrypoint that `Zune.exe` passes execution to. Along with the Iris startup code, it primarily contains code that glues some parts of the UI and the business logic together. However, note that the majority of the UI logic was written as embedded UIX scripts in the various resource DLLs. In Zune desktop 4.x, the scripts are compiled to custom bytecode.

## How can I use Microsoft Iris?
Despite being very powerful and flexible compared to other UI frameworks of the time, Microsoft never released Iris for public use. [`Microsoft.Iris.Application.VerifyTrustedEnvironment()`](https://github.com/ZuneDev/MicrosoftIris/blob/ab33f58c69275df5cb31b557887b8853925371c9/UIX/Microsoft/Iris/Application.cs#L488-L510) is called on startup in Iris applications to verify that the calling assembly is signed by Microsoft.

This check can be patched out with relative ease, though it should be done with caution. Distributing patched binaries violates U.S. copyright law and may land you in hot water.

## History
### 2.x
Microsoft Iris first appeared in the release of version 2.0 of the Zune desktop software. No pre-built controls were included at all, with even primitives like buttons requiring consumer to build from scratch. Compiled UIX also did not exist at this time. Instead, Iris would parse and interpret UIX XML at runtime. UIX XML syntax closely resembled [US-20090327876-A1](./US-20090327876-A1.pdf), a patent filed by Microsoft at the end of 2009, and [Media Center Markup Language (MCML)](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/windows-media-center-sdk/bb189388(v=msdn.10)) as used by [Windows Media Center](https://en.wikipedia.org/wiki/Windows_Media_Center).

### 3.x
Iris 3.0 released with Zune 3.0 in late 2008. It was the most significant update to Iris as it introduced the UIX compiler; rather than parsing XML at runtime, Iris could compile UIX files at buildtime into [UIB 3](./compiled-uix-3.md) a proprietary format that resembled custom bytecode. The shipped version of `UIX.dll` includes the original UIX XML interpreter and the binary UIX compiler for UIB 3, although neither are used by the Zune software at runtime.

The UIX XML syntax was changed sightly from the source included in Zune 2.0. For example:

- Assignments and expressions in bracketless conditionals are no longer wrapped in square brackets. 
    - `HyperlinkRepeater.Source = [Text.Fragments];` to `HyperlinkRepeater.Source = Text.Fragments;`
    - `if ([FragmentMouseFocus.Value])` to `if (FragmentMouseFocus.Value)`
- Canonical instances are accessed as if they are static members using dot syntax.
    ```xml
    <toptoolbar:TopToolbar Name="TopToolbar" Shell="{Shell}" FocusOrder="1">
        <LayoutInput>
            <DockLayoutInput Position="Top"/>
        </LayoutInput>
    </toptoolbar:TopToolbar>
    ```
    becomes
    ```xml
    <toptoolbar:TopToolbar Name="TopToolbar" Shell="{Shell}" FocusOrder="1" LayoutInput="{DockLayoutInput.Top}"/>
    ```

### 4.x
Iris 4.0 was a minor update released with Zune 4.0 in 2009, which utilized a new version of the compiler and [UIB 4](./compiled-uix.md) to reduce the size and overhead of UIX.

Iris 4 also included `UIXcontrols.dll`, a simple library containing some .NET code and implementations of basic controls, such as buttons and scrollbars, and some higher level but still primitive building blocks.

Zune 4.8.2345.0 shipped the last known version of Iris, with the same assembly version as Zune.