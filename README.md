# NLP++ analyzer template

A starting point for an [NLP++](https://visualtext.org/nlp/) text analyzer, with continuous
integration that runs it on every push.

NLP++ is a programming language for natural language processing. An analyzer is a sequence of
passes; each pass matches pattern rules against a parse tree and runs code when a rule fires, and a
built-in knowledge base holds what the analyzer learns as it reads.

## Start a project

1. Click **Use this template** above, then **Create a new repository**.
2. Add the topic **`nlp-plus-plus`** to your repository (the gear next to *About*) so other NLP++
   projects can find it.
3. Clone it and open the folder in VS Code with the
   [VisualText extension](https://marketplace.visualstudio.com/items?itemName=dehilster.nlp)
   installed. The extension installs the NLP engine and its help.

## What is in it

```
spec/analyzer.seq   the pass sequence
spec/*.nlp          the passes
input/              text files to analyze
kb/user/            knowledge base sources: .dict dictionaries and .kbb knowledge bases
```

The analyzer builds a knowledge base and exports it as JSON:

- **`kbinit`** creates a `kb` concept off the knowledge-base root, keeps it in the global variable
  `G("kb")`, and fills in a small example (`animals` with `dog` and `cat`). Replace the example with
  concepts your grammar passes find in the text.
- **`output`** saves the knowledge base with `SaveKB` and exports it with `JsonKB`.
- **`KBFuncs`** is VisualText's shared knowledge-base function library, where `SaveKB` and `JsonKB`
  come from. Leave it as it is and put your own functions in a separate `@DECL` pass after it.

Add your grammar passes between `kbinit` and `output`. Results appear in `input/<file>_log/`.

## Run it

In VS Code, select a file in `input/` and run the analyzer from the VisualText panel.

From the command line:

```sh
nlp -ANA <this folder> -IN <this folder>/input/text.txt -WORK <engine folder>
```

`-WORK` is the engine's own folder (the one containing the `nlp` executable and its `data/` folder),
not the analyzer's. With the VS Code extension installed it is
`~/.vscode/extensions/dehilster.nlp-<version>/nlp-engine`.

## Continuous integration

`.github/workflows/analyze.yml` installs the latest NLP engine release on every push and pull request,
runs the analyzer over every `input/*.txt`, and fails if the engine reports an error. Each file's
output log is kept as a downloadable artifact named `output-logs`.

## Learn more

- [VisualText](https://visualtext.org) and the [NLP++ textbook](https://book.visualtext.org)
- [NLP engine](https://github.com/VisualText/nlp-engine)
- More templates (dates, addresses, emails, telephone numbers, URLs, English parsing) ship with the
  VS Code extension.

## License

MIT.
