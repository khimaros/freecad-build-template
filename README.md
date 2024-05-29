# FREECAD BUILD

this template repository is configured to use [freecad-build](https://github.com/khimaros/freecad-build). the included `Makefile` contains targets to generate STEP, GCode, and 3MF for any FreeCAD FCStd files in the repository.

## quick start

follow the installation instructions for [freecad-build](https://github.com/khimaros/freecad-build)

make a simple change to `examples/example-part.FCStd` in this repository and save it

display a visual diff of the change:

```shell
git difftool -x freecad-difftool -d
```

## limitations

- if a part is modified but its parent assembly is not subsequently saved, the assembly diff will be skipped by git

## preparation

add the contents of [gitconfig-example](gitconfig-example) to `~/.gitconfig`:

```shell
cat gitconfig-example >> ~/.gitconfig
```

NOTE: alternatively, add to `.git/config` for each repo you want to enable this in.

## usage

### diff

NOTE: the following assumes that at least one FCStd file has been mmodified, otherwise nothing will happen:

visually diff all modified FreeCAD parts and assemblies in the repository:

```shell
git difftool -t freecad -d
```

after a wait relative to the complexity of the exported part, a FreeCAD window will open with at least two and at most four features.

**Previous**: the exported part from HEAD

**Modified**: the exported part including modifications in your worktree

**Additions**: any geometry which was added by your modifications (green)

**Subtractions**: any geometry which was removed by your modifications (red)

NOTE: the first two features are always present, the others are present only if the diff is non-empty

### export

export STEP files for any present FCStd files:

```shell
make step
```

build a specific STEP file:

```shell
make examples/example-assembly.step
```

NOTE: as usual with `make`, target files will only be rebuilt on subsequent runs if the source file has changed.

generate 3MF or Gcode for a part:

```shell
make examples/example-assembly.3mf
make examples/example-assembly.gcode
```

other file formats are supported by `freecad-export`, but would need to be added to the `Makefile`.

here's an example using `freecad-export` directly:

```shell
freecad-export examples/example-assembly.FCStd examples/example-assembly.iges
```

## troubleshooting

check the log output on the console, make sure there are no `recompute` errors.

inspect the **Modified** and **Previous** features to make sure they look as expected.

run the build in interactive mode, `export INTERACTIVE=1` before invoking
