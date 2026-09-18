# CNU

## Overview

CNU is the declarative interface and configuration language used by dreaOS to describe windows, visual elements, controls, actions, menus, positioning, state and other user-interface behavior.

CNU source files use the `.cnu` extension.

CNU is designed around readable declarations rather than a traditional imperative program structure. A CNU document describes what an interface contains, how it is presented, and what should happen when the user interacts with it.

## Basic structure

A CNU document is normally organized as nested declarations:

```cnu
_COMPONENT(
  PROPERTY(value)

  CHILD(
    PROPERTY(value)
    ACTION(...)
  )
)
```

Parentheses define declaration scopes. Nested declarations belong to the component immediately above them.

## Naming

CNU identifiers commonly use uppercase names for structural declarations and properties:

```cnu
_WINDOW(
  _POSITION(...)
  _COLOR(...)
)
```

Custom identifiers may also use mixed casing when they represent a named component, resource, menu or action.

## Comments

CNU supports line comments using `--`:

```cnu
-- This is a comment
```

Comments are ignored by the interface parser.

## Values

CNU values can be strings, identifiers, numbers, dimensions, colors, states, actions or lists.

Examples:

```cnu
_COLOR(WHITE)
_DEFAULT-Size:AllScreen*size:16:10
_TEXT("welcå")
size:8:5
```

### Strings

Quoted strings are used for visible text and other literal values:

```cnu
_TEXT("welcå")
_TEXT("This is the dreaOS installer Window")
```

### Lists

Multiple values can be grouped with commas or brackets depending on the declaration:

```cnu
_MAINPALLETTE(LIGHT-GRAY, BLACK, WHITE)

_OPTIONS[
  OPTION_ONE
  OPTION_TWO
]
```

## Windows

The default window declaration is represented with `DEFAULT_window`.

Example:

```cnu
_HDDR34_WINDOW{
  DEFAULT_window(
    _DEFAULT-Size:AllScreen*size:16:10
  )
}
```

The window can contain controls, menus, content, keyboard commands and state declarations.

## Window controls

A window can define controls such as close, minimize and expand:

```cnu
buttonClose(
  _icon("X"Color:Red)
  _ACTION(CloseWindow)
)

buttonMinimize(
  _icon("-"Color:Yellow)
  _ACTION(MinimizeWindow)
)

buttonExpande(
  _icon("<>"Color:Green)
  _ACTION(OpenMenu"WindowPositionMenu")
)
```

Actions are declarative. The CNU runtime interprets them and performs the corresponding system or window-manager operation.

## Window positioning

CNU can describe predefined window positions using an options menu.

Example:

```cnu
WindowPositionMenu(
  _OPTIONS[
    O1*IPositionScreenSquareTopLeft8-5Size
    02*IPositionScreenSquareUnderLeft8-5Size
    03*IPositionScreenSquareTopRight8-5Size
    04*IPositionScreenSquareUnderRight8-5Size

    05*IPositionHalfVScreenLeft8:10Size
    06*IPositionHalfVScreenRight8:10Size

    07*IPositionHalfHScreenTop16:5Size
    08*IPositionHalfHScreenUnder16:5Size
  ]
)
```

For the dreaOS default window layout:

| Position | Format |
|---|---:|
| Top-left quarter | 8:5 |
| Bottom-left quarter | 8:5 |
| Top-right quarter | 8:5 |
| Bottom-right quarter | 8:5 |
| Left half | 8:10 |
| Right half | 8:10 |
| Top half | 16:5 |
| Bottom half | 16:5 |
| Default window | 16:10 |

These are aspect-ratio declarations used by the dreaOS window-position system, not fixed pixel resolutions.

## Actions

Actions are introduced with `_ACTION`:

```cnu
_ACTION(CloseWindow)
_ACTION(MinimizeWindow)
_ACTION(OpenMenu"WindowPositionMenu")
```

Multiple operations can be grouped:

```cnu
_ACTION(
  MinimizeWindow,
  SaveLastState!,
  SaveAppOpenedInDock
)
```

## User interaction

CNU can branch behavior from user actions:

```cnu
if userAction:ClickButton(
  return InstallatorWindowInfoMenu
)
```

For controls with state-dependent behavior:

```cnu
if userAction:ClickButton*IfIsPaused*
  .ContinueInstallation#continue*
```

The exact syntax may evolve with the CNU runtime.

## State

CNU can describe persistent UI state.

Example:

```cnu
_PAUSE-ACTION.*INSTALLATION
_SAVESTEP-&INFOTYPED
_STATE*.Paused*ChangeDESIGN("▶️")
```

A pause-capable installer can therefore store the current installation state and later continue from that state.

## Menus

Menus are declared as nested option collections:

```cnu
menucdd{
  _OPTIONS[
    *(Ågg kåk PC)*
    *(Ôfegen såsten giådrösen..)*.*OPTION-DISABLED.*ACTION(OpenApp.*SystemSettings)
    *(Rêdgtad)*.*ACTION(RestartSystem)
    *(Vrågen)*.*ACTION(ShutDownSystem)
    *(Süspend)*.*ACTION(LockScreen)
  ]
}
```

Options can be disabled independently and can execute actions when selected.

## Visual elements

CNU can define colors, palettes, typography, animations and imported visual designs.

Examples:

```cnu
_COLOR_COLOR(WHITE)
_MAINPALLETTE(LIGHT-GRAY, BLACK, WHITE)

_FIRST_LINE(
  _COLOR(LIGHTGREEN#000010)
  _ANIMATION(import LineUpAnimation FROM *AnimationsRepo^^ -&Use)
)

_SECOND_LINE(
  _COLOR(LIGHTBLUE#1000000)
  _ANIMATION(use LineUpAnimation)
)
```

## Text

Text elements can contain typography, position and per-character styling:

```cnu
_TEXT(
  "welcå"
  --typography(import manuscaligraphy FROM *leterfontRepo^^ -&Use)
  --position(UnderCIRCLE4px)
)
```

## Imported resources

CNU can reference resources from named repositories or resource collections:

```cnu
import LineUpAnimation FROM *AnimationsRepo^^ -&Use
import multicolordesignforBall FROM *DesignsRepo^^ -&Use
import macOScursor FROM *cursorRepo^^ -&Use
```

The runtime resolves these references and makes the imported resource available to the declaration.

## Cursor

A cursor can be defined in its own CNU file and referenced by another interface:

```cnu
_CURSOR(
  --use */cursor/cursor.cnu
)
```

Example cursor declaration:

```cnu
-design(import macOScursor FROM *cursorRepo^^ -&Use)
  -layerscreen(top:1)
```

## Keyboard commands

CNU can associate keyboard shortcuts with actions:

```cnu
_KEYCOMMANDS(
  '⌘F'*ACTION(OpenWindow*.CMD)
  '⊞L'*ACTION(OpenWindow*.CMD)
)
```

## Resizable windows

A window can expose a resize interaction:

```cnu
_ACTION{
  if userAction:HoldButton(
    resizeWindow(allSizes)SaveLastPositionAfterHoldEnds()
  )
}
```

This allows the runtime to resize the window and preserve the resulting position.

## Example: dreaOS installer interface

A simplified example combining the main concepts:

```cnu
_MAGIC3-VISUALINTERFACE(
  RIGHT_SCREEN(
    DESIGN(
      _COLOR_COLOR(WHITE)

      _HDDR34_WINDOW{
        DEFAULT_window(
          _DEFAULT-Size:AllScreen*size:16:10

          buttonExpande(
            _icon("<>"Color:Green)
            _ACTION(OpenMenu"WindowPositionMenu")
          )

          WindowPositionMenu(
            _OPTIONS[
              O1*IPositionScreenSquareTopLeft8-5Size
              02*IPositionScreenSquareUnderLeft8-5Size
              03*IPositionScreenSquareTopRight8-5Size
              04*IPositionScreenSquareUnderRight8-5Size
              05*IPositionHalfVScreenLeft8:10Size
              06*IPositionHalfVScreenRight8:10Size
              07*IPositionHalfHScreenTop16:5Size
              08*IPositionHalfHScreenUnder16:5Size
            ]
          )
        )
      }
    )
  )
)
```

## File extension

Use:

```
.cnu
```

For example:

```
welcome.cnu
cursor.cnu
installer.cnu
```

## Status

CNU is an evolving dreaOS language. Syntax and runtime behavior may change as the dreaOS interface system develops.
