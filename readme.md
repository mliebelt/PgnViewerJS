# PgnViewerJS

PgnViewerJS is a simple JavaScript implementation to show PGN files (Portable Game Notation == Chess)
in a web page. PgnViewerJS does not write everything anew, but it uses the following libraries:

* [chess.js](https://github.com/jhlywa/chess.js) Base library to model chess in JavaScript
* [Chessground](https://github.com/ornicar/chessground) The chessboard used by lichess.org
* [pegjs](https://github.com/pegjs/pegjs) Parser Generator implemented in Javascript.

See the section Acknowledgments for more supporting libraries.

See the working examples on my new GitHub Pages site [PgnViewerJS](http://mliebelt.github.io/PgnViewerJS/), especially the [Configuration Builder](http://mliebelt.github.io/PgnViewerJS/config/config.html) that demonstrates all parameters in a running example.

## Getting Started

The easiest way to use it is to install it by using NPM: `npm install @mliebelt/pgn-viewer`. You will find there the diretory `lib` that contains everything. Copy that onto your webserver.

### Example Screenshots

The following are example screenshots with the configuration set below, to show the rich possibilities of PgnViewerJS.

![Theme Sportverlag](screenshots/sportverlag.png)
![Edit with variations](screenshots/edit-variation.png)
![Viewer](screenshots/view-layout-left.png)

The screenshots show from left to right:

* `mode: board; theme: sportverlag; pieceStyle: uscf`
* `mode: edit; orientation: black; locale: de` with comments
* `mode: view; layout: 'left'` with comments

Go to the [github.io pages](http://mliebelt.github.io/PgnViewerJS/) (documentation), where the current (and former) versions are available.

### Features

This implementation has the following features:

* Allows to show one chess game complete, with a lot of different styles, themes, tuning, ...
* Allows to play through the games forward and backward, including variations.
* Allows to play from a legal position only legal moves, and adds these moves to the notation (in a different style)
* Allows to play through by
  * clicking on moves
  * clicking on next and previous button
  * clicking on play button
  * using cursor left and right on the keyboard
* Allows to add moves to a game, when in the right 'mode'.
* Knows all PGN notation elements, and knows how to render them.

### UI modes

There are four different kind of usages:

* `PGNV.pgnView` renders the whole game, and allows to play it through, jump to any position.
* `PGNV.pgnEdit` is a superset of pgnView, that allows to additional add variations, change the order of main line and variations, and allows all other kind of interactions that are possible: adding comments, PGN notation elements, whatever.
* `PGNV.pgnBoard` will just show a position without any interaction possible.
* `PGNV.pgnPrint` will just show a whole chess game, without any interaction possible.

There is at the moment no way to save a game that was edited in `pgnEdit` mode. But you may at least copy the whole notation, and insert it again in the HTML code of your web page.

## Running the tests

* Run `npm test`. This runs the tests for the module `pgn-reader`.

## Deployment

### From Release

* Copy the files from directory `lib` into a web server.
* Create new HTML files with the corresponding head and body.

### From Clone

* Clone the repository.
* Run `npm install` from the root, to ensure are tools are installed.
* Run `npm build` to both modules (first pgn-reader, then pgn-viewer) build a distribution.
* Copy the files from the directory `modules/pgn-viewer/lib`.
* Create new HTML files with the corresponding head and body.

### Using the viewer

#### Standalone script

To use the viewer in an HTML page, you have to do the following steps:

* Include the necessary library `pgnv.js` and the corresponding resources `locales` and `pgnv-assets`.
* Have Fontawesome referenced.
* Include the necessary div container for rendering of the  board and the moves.
* Include JavaScript code inside your HTML page to call the viewer.

So a rough template will look like:

```html
<!DOCTYPE html>
    <head>
        <script src="js/pgnv.js" type="text/javascript" ></script>
    </head>
    <body>
        <div id="board"></div>
        <script>
    	PGNV.pgnView('board',{ pgn: '1. e4 e5 2. Nf3 Nc6 3. Bb5', pieceStyle: 'merida' });
        </script>
    </body>
</html>
```

#### ReactJS

```jsx
import React, {useLayoutEffect} from 'react'
import Children from 'react-children-utilities'
import * as uuid from 'uuid'
import { pgnView } from '@mliebelt/pgn-viewer'

function PGNViewer(props) {
  const gameDecription = Children.onlyText(props.children)
  const id = 'board-' + uuid.v4()

  useLayoutEffect(() => {
    pgnView(id,
      {
        pgn: gameDecription,
        timerTime: '1',
        locale: 'pl',
        startPlay: 1,
        showResult: true,
        boardSize: '340',
        showFen: true,
        pieceStyle: 'chesscom'
      }
    )
  })

  return (
    <div id={id}></div>
  )
}

// Usage

<PGNViewer>
[Event "F/S Return Match"]
[Site "Belgrade"]
[Date "1992.11.04"]
[Round "29"]
[White "Fischer, Robert J."]
[Black "Spassky, Boris V."]
[Result "1/2-1/2"]

1. e4 e5 2. Nf3 Nc6 3. Bb5 a6 4. Ba4 Nf6 5. O-O Be7 6. Re1 b5 7. Bb3 d6 8. c3 O-O 9. h3 Nb8  10. d4 Nbd7
11. c4 c6 12. cxb5 axb5 13. Nc3 Bb7 14. Bg5 b4 15. Nb1 h6 16. Bh4 c5 17. dxe5 Nxe4 18. Bxe7 Qxe7 19. exd6 Qf6 
20. Nbd2 Nxd6 21. Nc4 Nxc4 22. Bxc4 Nb6 23. Ne5 Rae8 24. Bxf7+ Rxf7 25. Nxf7 Rxe1+ 26. Qxe1 Kxf7 27. Qe3 Qg5 
28. Qxg5 hxg5 29. b3 Ke6 30. a3 Kd6 31. axb4 cxb4 32. Ra5 Nd5 33. f3 Bc8 34. Kf2 Bf5 35. Ra7 g6 36. Ra6+ Kc5 
37. Ke1 Nf4 38. g3 Nxh3 39. Kd2 Kb5 40. Rd6 Kc5 41. Ra6 Nf2 42. g4 Bd3 43. Re6 1/2-1/2
</PGNViewer>
```

## Built With

* [NPM](https://www.npmjs.com/) For libraries, building, ...
* [Webpack](https://webpack.js.org/) For building optimized bundle.

## Contributing

If you find something strange (bug), expect some feature (feature) or just want to comment on anything, please file first a ticket in Github.

If you want to help in implementing something, clone the repository, change whatever you want to, and provide a pull request that I can look at.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/mliebelt/PgnViewerJS/tags).

From the version 1.0.0 on, the whole package can be downloaded / installed by using NPM:

* Download: `npm pack @mliebelt/pgn-viewer` Results in download of a file `mliebelt-pgn-viewer-1.3.0.tgz`, that contains in the directory `package/lib` all resources needed.
* Install: `npm install @mliebelt/pgn-viewer` as part of some other application. You will find then the files in the directory `node_modules/@mliebelt/pgn-viewer/lib`.

The older versions are available for download from the links below.

* [Version 0.9.8](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.8.zip): Start play parameter, allow arrows and circles from Chessground, color marker for player at move, option for result in PGN notation. 
* [Version 0.9.7](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.7.zip): UI with Chessground, small improvements, promotion.
* [Version 0.9.6](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.6.zip): A lot of fixes, multi-line notation, stability.
* [Version 0.9.5](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.5.zip): Some more fixes, stability.
* [Version 0.9.4](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.4.zip): Finished edit mode, cleanup, some more fixes.
* [Version 0.9.3](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.3.zip): Allows special markup, some more bug fixes.
* [Version 0.9.2](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.2.zip): Some more bug fixes, examples to all issues at [GitHub](https://github.com/mliebelt/PgnViewerJS/issues), added Changelog.md, started restructuring the sources.
* [Version 0.9.1](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.1.zip): Some bug fixes, examples to all issues at [GitHub](https://github.com/mliebelt/PgnViewerJS/issues),
  some additional examples and a lot of fixes in the documentation.
* [Version 0.9.0](https://s3.eu-central-1.amazonaws.com/pgnviewerjs/releases/PgnViewerJS-0.9.0.zip): Nearly feature complete, roughly documented, stable enough to play with it.

## Authors

* Markus Liebelt - [mliebelt](https://github.com/mliebelt)

## License

PgnViewerJS is licensed under the Apache license 2.0, see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgments

We use the following libraries in the implementation:

* [chess.js](https://github.com/jhlywa/chess.js) Base library to model chess in JavaScript
* [Chessground](https://github.com/ornicar/chessground) The chessboard used by lichess.org
* [pegjs](https://github.com/pegjs/pegjs) Parser Generator implemented in Javascript.
* [Mousetrap](https://craig.is/killing/mice) Binds keys to actions in the UI
* [roddeh-i18n](http://i18njs.com//) Internationalization (== i18n) library to translate chess into current 18 languages. Strings are inlined in the library.
* [Timer.js](https://github.com/fschaefer/Timer.js) Small timer for automatic moves
* [FontAwesome](http://fontawesome.io/): Nice icon font used for some buttons
* [PGN Specification](http://www.saremba.de/chessgml/standards/pgn/pgn-complete.htm): PGN (Portable Game Notation) specification
* [NAG Specification](http://en.wikipedia.org/wiki/Numeric_Annotation_Glyphs) Definition of the NAGs (Numeric Annotation Glyphs)

Thank you a lot to all contributors of issues.

### Known Usage

* [PgnJS](https://github.com/xeyownt/mwpgnjs) Plugin for Mediawiki that uses PgnViewerJS to display  chess games.
* [PgnViewerJS-WP](https://github.com/mliebelt/PgnViewerJS-WP) Plugin for Wordpress to show games with the help of PgnViewerJS
