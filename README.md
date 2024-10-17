<h1>
<img src='./proviewer.svg'
    style='position: relative; top: 7px; padding: 0px; filter: brightness(5) sepia(1) hue-rotate(180deg) saturate(6)' 
    alt='proviewer-logo' width='30' height='35'/>
ProViewer
</h1>

![](https://img.shields.io/badge/npm-1.0.6-yellow)
![](https://img.shields.io/badge/minzipped_size-18.8KB-blue)
![](https://img.shields.io/badge/three.js-r169-green)
![](https://img.shields.io/badge/license-mit-red)

## Overview
**ProViewer** is a 3D model viewer for the safe and fast rendering of the most common 3D model files using the [three.js](https://threejs.org/) library. This viewer uses *encryption and decryption* algorithms for the safe transaction of model files on the Internet and protection of model files from illegal copying. Currently, it supports a total of 16 different model file formats, with more to come in the future. Additionally, it will soon offer special shaders and post-processing plugins for realistic rendering and further analysis.

## Supported File Formats
**3DS, 3MF, AMF, DAE, FBX, MD2, GLB, GLTF, KMZ, OBJ(MTL), PLY, STL, SVG, VTK, VOX, WRL**

The figures below show the rendered images after loading the sample models. Each of the first 8 images resulted from rendering one model file per viewport, and the remaining 18 images were created at the same time using multiple grid viewports.

<div>
<img src="images/single_viewport.jpg" width="100%" style="margin: 1px" alt="single_viewport">
<img src="images/multiple_viewports.jpg" width="99%" style="margin: 1px" alt="multiple_viewports">
</div>

## Proviewer Test
You can run this viewer at the website: https://nova-graphix.github.io/proviewer/.

## Installation
You can install this viewer (called **proviewer**) to your project by executing:
```sh
npm install proviewer
```
Then, you should include **proviewer** to your project as follows:
```js
import { renderModels } from 'proviewer';
renderModels( options );
```
where the **renderModels()** is a JavaScript function that renders models, and the **options** is a JavaScript object that includes the model URL, CSS-style, background image, and animation variables if available.

## Basic Usage
- **Rendering models by dragging and dropping:**
    With the Javascript code below, you can view your model by dragging and dropping your model file on the viewport created by the function **renderModels()**.
    ```js
    import { renderModels } from 'proviewer';
    renderModels();
    ```

- **Rendering a specific model:**
    The following code shows an example of rendering a specific model using **renderModels()**. In the code below, **id** is the ID of the \<div> HTML element for the model, **style** is the CSS codes that style the \<div> element, **img** is used for the background image of this viewer, and **url** is for model URLs. You can write one or more model URLs in this **url**. In addition, you can bundle multiple models into a *zip* file and enter it into this **url**. Note that the .zip file can contain other .zip files.
    ```js
    import { renderModels } from 'proviewer';
    renderModels({
        // id: id of <div>
        id:    'model',
        // style: CSS styling
        style: 'width:800px; height:600px; ....',
        // img: (eg) '0x87ceeb' or 'skyblue' also possible
        img:   'path/to/image.jpg',
        // url: your model (.glb)
        url:   'path/to/model.glb',
    });
    ```
    ```js
    import { renderModels } from 'proviewer';
    renderModels({
        // id: internally generated if not defined
        id:    'model',
        // style: CSS styling
        style: 'width:800px; height:600px; ....',
        // img: 6 images for cube texture
        img: `[
            "path/to/px.png", "path/to/nx.png",
            "path/to/py.png", "path/to/ny.png",
            "path/to/pz.png", "path/to/nz.png"
        ]`,
        // url: .zip contains multiple models
        url:   'path/to/models.zip',
    });
    ```

- **Rendering multiple models in grid viewports:**
    Multiple models can be placed in grid viewports using just one WebGL renderer. In the code below, **id** is the ID of the \<div> HTML element as a model container, and **children** are child elements of this \<div> element, each of which renders a model within its viewport.
    ```js
    import { renderModels } from 'proviewer';
    const style = 'width: 300px; height: 300px';
    renderModels({
        id: 'models',
        children: [
            { style: style, url: "path/to/model_01.glb" },
            { style: style, url: "path/to/model_02.zip" },
            { style: style, url: "path/to/model_03.stl" },
            ....
        ]
    });
    ```
    Note that the \<canvas> HTML element is created internally, and all models are drawn on this \<canvas> element.

- **Rendering models with animation data:**
    For animation models, input data such as the model's URL, animation clip name, etc. must be entered in JSON format in the *animate* property as in the following code for animation actions.
    ```js
    import { renderModels } from 'proviewer';
    import { LoopRepeat } from 'three';
    renderModels({
        id: 'models',
        img: 'path/to/image.png',
        // url: available in json format or in string format
        url: `[
            "path/to/model_01.glb",
            "path/to/model_02.zip"
        ]`,
        // animate: required to be written in json format
        animate: `[
          {
            "url": "path/to/model_01.glb",
            "clip": (eg) "jump",
            "duration": 1,
            "loopMode": ${LoopRepeat},
            "repetitions": ${Number.MAX_SAFE_INTEGER},
            "combine": "crossFade"
          },
          {
            "url": "path/to/model_02.zip",
            "clip": (eg) "Run"
          }
        ]`,
    });
    ```
    In the above *animate* code, *url* is required for multiple models, but not for single model. *clip* is required data, such as "jump" or an index pointing to "jump", and the remaining *duration*, *loopMode*, *repetitions*, and *combine* are all optional.

- **Encryption and decryption of model files:**
    The function **openEncryptFiles()** encrypts model files with a model encryption key to prevent illegal copying on the Internet. The encrypted files can return to their original file state through the function **openDecryptFiles()** with a model decryption key. A file dialog box is opened by these functions. Then, the user selects the directory where the files to be encrypted or decrypted exist. When you press the OK button, the encryption or decryption process begins. Note that two secret keys for encryption and decryption are provided only to paid users.
    ```js
    import { openEncryptFiles, openDecryptFiles } from 'proviewerx';

    // NOTE*: two model secret keys are provided only to paid users
    openEncryptFiles( settings.MODEL_ENC_KEY );
    openDecryptFiles( settings.MODEL_DEC_KEY );
    ```
    In the code above, **settings** is an object that plays the same role as *process.env*.

## Model Inspection (in progress)
- **lights**<br>
    Press the **key l** on your keyboard to increase the intensity of light sources illuminating the model. To decrease the light intensity, hold down the **shift key** and press the **key l**.
- **normals**<br>
    Press the **key n** to map the normal vectors to RGB colors. Press it again to remove the mapping.
- **animation**<br>
    Press the **key p** to play the model animation. If you want to see the next animation, press the **key p** again. To pause the animation, press the **key o**.
- **wireframe**<br>
    Press the **key w** to view the model's geometry as a wireframe. Press it again to go back.
- **background**<br>
    Press the **key b** on your keyboard. Then, you can see that the background image changes. There are approximately 5 background images available. Unfortunately, in order to minimize the bundle size of ProViewer, this is no longer supported in the current version. If requested, it can be provided separately.
- **matcap**<br>
    Press the **key m** to apply the *matcap* (material capture) shader. Press it again to remove the shader. Unfortunately, it is not supported in the current version. However, it can be provided separately upon request.

## Contact Us
Please contact us at info@nova-graphix.com for any questions or suggestions.
- Website: https://www.nova-graphix.com
- LinkedIn: https://www.linkedin.com/company/novagraphix/
- Facebook: https://www.facebook.com/NovaGraphixCo
- YouTube: https://www.youtube.com/@3D-novagraphix

## License
This project is licensed under the MIT License.
