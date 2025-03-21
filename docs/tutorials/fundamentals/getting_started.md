---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

(launch)=
# How to launch napari

This tutorial assumes you have already installed napari.
For help with installation see our [installation tutorial](napari-installation).

This tutorial will teach you all the different ways to launch `napari`.
At the end of the tutorial you should be able to launch `napari` and see the viewer your favorite way.

## Launching `napari`

There are four ways to launch the `napari` viewer:

- [command line](launch-command)
- [python script](launch-script)
- [IPython console](launch-ipython)
- [jupyter notebook](launch-jupyter)

All four of these methods will launch the same `napari` viewer
but depending on your use-case different ones may be preferable.

(launch-command)=

### Command line usage

To launch `napari` from the command line simply run

```sh
napari
```

This command will launch an empty viewer:

```{code-cell} ipython3
:tags: [remove-input]
import napari
from napari.utils import nbscreenshot


viewer = napari.Viewer()

nbscreenshot(viewer, alt_text="Screenshot of an empty napari viewer, right after launching.")
```

Once you have the viewer open you can add images through the `File -> Open` dropdown menu
or by dragging and dropping images directly on the viewer. We currently only support files that can be read with [`skimage.io.imread`](https://scikit-image.org/docs/dev/api/skimage.io.html#skimage.io.imread),
such as `tif`, `png`, and `jpg`.  We plan on adding support for more exotic file types shortly - see [issue #379](https://github.com/napari/napari/issues/379) for discussion. Finally, you can use the `File -> New Image from Clipboard` menu item to make a new `Image` layer from an image (or URL to an image) copied to your Clipboard (keybinding {kbd}`Command/Ctrl+N`).

You can also directly load an image into the viewer from the command line by passing the path to the image as an argument as follows

```sh
napari my_image.png
```

If the image is `RGB` or `RGBA` use the `-r` or `--rgb` flag.

Here's an example of a viewer with an RGB image, the `astronaut` sample image.

```{code-cell} ipython3
:tags: [remove-input]
import napari
from napari.utils import nbscreenshot
from skimage.data import astronaut


viewer = napari.Viewer()
viewer.add_image(astronaut())

nbscreenshot(viewer, alt_text="Screenshot of an napari viewer showing the scikit-image sample image `astronaut`, which is an RGB image of the astronaut Eileen Collins.")
```

Launching `napari` directly from the command line is the simplest and fastest way to open the viewer,
but it doesn't allow you to preprocess your images before opening them.
It is also currently not possible to save images or other layer types directly from the viewer,
but we'll be adding support for this functionality soon as discussed in [#379](https://github.com/napari/napari/issues/379).

If you wish to interact with your open viewer programmatically you can open an IPython
console via **Window** > **console** or clicking the 'Show/Hide IPython console' button
within the [viewer buttons](viewer-layout).

(launch-script)=

### Python script usage

To launch `napari` from a python script, inside your script you can import `napari`,
then create a {class}`Viewer<napari.Viewer>` and {class}`Image<napari.layers.Image>`
layer by adding some image data, using {func}`imshow<napari.imshow>`.
The {class}`Viewer<napari.Viewer>` is representative of the napari viewer GUI
you launch and stores all the data you add to napari. The
{class}`Image<napari.layers.Image>` will store information about the image data
you added.

For example, to add an image and print the shape of the image layer data,
you can use:

```python
# import sample data
from skimage.data import cells3d

import napari

# create a `Viewer` and `Image` layer here
viewer, image_layer = napari.imshow(cells3d())

# print shape of image data
print(image_layer.data.shape)

# start the event loop and show the viewer
napari.run()
```

Note that {func}`imshow<napari.imshow>` is a convenience function that is
equivalent to:

```python
# import sample data
from skimage.data import cells3d

import napari

viewer = napari.Viewer()
image_layer = viewer.add_image(cells3d())
```

You can now run your script from the command line to launch the viewer with your data:

```sh
python my_example_script.py
```

The [examples gallery](../../gallery) consists of code examples which can be
downloaded as `.py` (and `.ipynb` files) and run as above.

![image: napari launched from a python script](../../_static/images/launch_script.png)

An advantage of launching `napari` from a python script
is that you can preprocess your images and add multiple layers before displaying the viewer.

As above, if you wish to interact with your open viewer programmatically you can open
an IPython console via **Window** > **console** or clicking the 'Show/Hide IPython
console' button within the [viewer buttons](viewer-layout).

(launch-ipython)=

### IPython console usage

To launch `napari` from an IPython console import `napari` and create a
{class}`Viewer<napari.Viewer>` and {class}`Image<napari.layers.Image>` object.

```python
# import sample data
from skimage.data import cells3d

import napari

# create a `Viewer` and `Image` layer here
viewer, image_layer = napari.imshow(cells3d())
```

`napari` will automatically use the interactive [`%gui qt` event
loop](https://ipython.readthedocs.io/en/stable/config/eventloops.html#integrating-with-gui-event-loops)
from IPython

![image: napari launched from ipython](../../_static/images/launch_ipython.png)

An advantage of launching napari from an IPython console
is that the you can continue to programmatically interact with the viewer from the IPython console,
including bidirectional communication, where code run in the console will update the current viewer
and where data changed in the GUI will be accessible in the console.

(launch-jupyter)=

### Jupyter notebook usage

You can also launch `napari` from a Jupyter notebook. The
[examples gallery](../../gallery), as mentioned above, can also be downloaded as
`.ipynb` which can be run from a Jupyter notebook.

Below, we launch the [notebook example](https://github.com/napari/napari/blob/main/examples/notebook.ipynb) from a Jupyter notebook.

![image: napari launched from a Jupyter notebook](../../_static/images/launch_jupyter.png)

Similar to launching from the IPython console,
an advantage of launching `napari` from a Jupyter notebook
is that you can continue to programmatically interact with the viewer from Jupyter notebook,
including bidirectional communication, where code run in the notebook will update the current viewer
and where data changed in the GUI will be accessible in the notebook.

## Next steps

To learn more about:

* how to use the napari viewer graphical user interface (GUI),
  checkout the [viewer tutorial](viewer-tutorial)
* how to use the napari viewer with different types of napari layers, see
  [layers at a glance](layers-glance)


## Solving common problems

### napari does not launch with `No Qt bindings could be found`

If you try to start napari and see an error message containing following (or similar) text:

```
ImportError: No Qt bindings could be found
```

First you need to validate if you have Qt bindings installed. You can do this by running:

```bash
pip list
```

And check if `PyQt5`, `PySide2`, `PyQt6` or `PySide6` are mentioned in the output as installed.

If there is no such entry, please install one of them following the instructions
in [Choosing a different Qt backend](choosing-qt-backend)

If some backend is installed but napari still does not start, please try to start it from command line:

```bash
LD_LIBRARY_PATH="" napari
```

If napari is successfully launched after this command, it means that on your machine
the `LD_LIBRARY_PATH` environment variable is set to a directory that contains
a Qt dynamic dependency that is incompatible with the one that napari uses.
As linkers first search in `LD_LIBRARY_PATH` directories, this may cause napari to crash.


If you want to debug this issue for your specific use case, you 
should start with manually importing the `qt` package in Python.
This will provide you with more information about problem. 

Such import may look like **one** of the following lines:

```python
from PyQt5 import QtWidgets
from PyQt6 import QtWidgets
from PySide2 import QtWidgets
from PySide6 import QtWidgets
```

Such import may raise an error like:

```
ImportError: .../site-packages/PyQt5/QtWidgets.abi3.so: undefined symbol: _ZdlPvm, version Qt_5
```

Then when you identify which file is causing the problem you can use `ldd` command
to check which libraries are used by this file:

```bash
ldd .../site-packages/PyQt5/QtWidgets.abi3.so
```
