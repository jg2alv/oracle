# oracle: pixel combinations and image counts

oracle explores pixel-color combinations using Python and Pillow. It supports RGB and black-and-white palettes, theoretical combination counts, and sample-based JPEG storage estimates.

## 1. Combinatorial model

For a grid containing `p` pixels, RGB mode considers `(256 ** 3) ** p` arrangements. Black-and-white mode considers `2 ** p`. The generator constructs Pillow image objects, and the CLI saves them as UUID-named JPEG files.

Storage estimation multiplies the mean encoded size of three sample JPEGs by the theoretical count. This is an approximation, not a measured total for the full image space.

## 2. Requirements

Python 3 and Pillow. `requirements.txt` pins `Pillow==7.2.0`; use a Python environment compatible with that dependency version.

## 3. Bounded example

From the repository root, create a new empty output directory and request the count for a one-pixel black-and-white grid:

```sh
mkdir oracle-demo
python3 app.py -s 1 1 -p oracle-demo ---black-white ---estimate-ammount
```

Expected count from the combinatorial model:

```text
2 images
```

To generate that same bounded case:

```sh
python3 app.py -s 1 1 -p oracle-demo ---black-white
```

Every invocation, including count-only mode, first creates, measures, and removes three temporary sample JPEGs. Generation then writes the final UUID-named JPEGs. The output directory must already exist and be writable.

The three-hyphen switches and the spelling of `---estimate-ammount` match the current parser. Keep demonstrations small, including estimation-only requests.

## 4. Limitations

- The number of arrangements grows exponentially; full RGB generation also materializes a large color pool.
- Rectangular-grid indexing is unreliable. Keep generation examples square and very small.
- JPEG is lossy, so distinct source arrangements do not guarantee distinct encoded image contents.
- The estimator runs before directory creation, so a missing output directory causes failure.
