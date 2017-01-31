[![Travis Build](https://travis-ci.org/Ks89/angular-modal-gallery.svg?branch=master)](https://travis-ci.org/Ks89/angular-modal-gallery)   [![Appveyor Build](https://ci.appveyor.com/api/projects/status/ikp5qqr9aci2s0ae/branch/master?svg=true)](https://ci.appveyor.com/project/Ks89/angular-modal-gallery/branch/master)   [![Known Vulnerabilities](https://snyk.io/test/github/ks89/angular-modal-gallery/badge.svg)](https://snyk.io/test/github/ks89/angular-modal-gallery)   [![david-dm Dependencies](https://david-dm.org/Ks89/angular-modal-gallery.svg)](https://david-dm.org/Ks89/angular-modal-gallery)   [![Code Climate](https://codeclimate.com/github/Ks89/angular-modal-gallery/badges/gpa.svg)](https://codeclimate.com/github/Ks89/angular-modal-gallery)   [![Code Climate Coverage](https://codeclimate.com/github/Ks89/angular-modal-gallery/badges/coverage.svg)](https://codeclimate.com/github/Ks89/angular-modal-gallery/coverage)   [![Coveralls Coverage](https://coveralls.io/repos/github/Ks89/angular-modal-gallery/badge.svg?branch=master)](https://coveralls.io/github/Ks89/angular-modal-gallery?branch=master)

[![bitHound Overall Score](https://www.bithound.io/github/Ks89/angular-modal-gallery/badges/score.svg)](https://www.bithound.io/github/Ks89/angular-modal-gallery)   [![bitHound Code](https://www.bithound.io/github/Ks89/angular-modal-gallery/badges/code.svg)](https://www.bithound.io/github/Ks89/angular-modal-gallery)   [![bitHound Dependencies](https://www.bithound.io/github/Ks89/angular-modal-gallery/badges/dependencies.svg)](https://www.bithound.io/github/Ks89/angular-modal-gallery/master/dependencies/npm)   [![bitHound Dev Dependencies](https://www.bithound.io/github/Ks89/angular-modal-gallery/badges/devDependencies.svg)](https://www.bithound.io/github/Ks89/angular-modal-gallery/master/dependencies/npm)

<br>
# Angular modal gallery

An Angular library to create an image gallery of clickable thumbnails. After a click, it displays a modal window with the large image.
It's also possible to navigate between these images with keyboard arrows and close it pressing `esc`.
More features coming soon!

This project was born as a fork of [THIS REPO](https://github.com/vimalavinisha/angular2-image-popup).

**This library uses [Semantic versioning 2.0.0](http://semver.org/) also known as 'semver'**


## **Official live DEMO**

*Image loading could be slow, because this demo is hosted on Github pages*

[GO HERE FOR A DEMO](https://ks89.github.io/angular-modal-gallery/)


## Installation

- `npm install --save angular-modal-gallery`


## News

- 01/30/2017 - 2.0.2 - angular-modal-gallery - [HERE](https://github.com/Ks89/angular-modal-gallery/releases)
- 01/29/2017 - 2.0.1 - angular-modal-gallery - [HERE](https://github.com/Ks89/angular-modal-gallery/releases)
- 01/29/2017 - 2.0.0 - angular-modal-gallery - [HERE](https://github.com/Ks89/angular-modal-gallery/releases)
- 01/28/2017 - 2.0.0-alpha.1 - angular-modal-gallery


## Features
- Angular module with a directive inside to display an image gallery
- AOT support
- Angular >=2 (it's also working with Angular 4 beta)
- Tested with Webpack2 + Treeshaking and also inside a Lazy loaded module
- and so on...


## How to use?

### Webpack

1. In your main module add:

    ```
    import { ModalGalleryModule } from 'angular-modal-gallery';     <--- import this library

    @NgModule({
      imports: [
        ...
        ModalGalleryModule.forRoot()    <--- add this in 'imports'
    ],
      declarations: [
        ...
      ],
      providers: [],
      bootstrap: [ ... ]
    })

    export class AppModule {
    ...
    }
    ```

2. In you component (file .ts)

    ```
    import { Component } from '@angular/core';

    @Component({
      selector: 'mmw-home-page',
      styleUrls: ['home.scss'],
      templateUrl: 'home.html'
    })
    export class HomeComponent {

      openModalWindow: boolean = false;
      imagePointer: number;
      images = [                                <--- array with your images thumbs + img + description
        {
          thumb: 'assets/images/gallery/thumbs/img1.jpg',  <-- thumb image path (this is only an example)
          img: 'assets/images/gallery/img1.jpg',           <-- image path (this is only an example)
          description: 'Image description 1'
        },
        {
          thumb: 'assets/images/gallery/thumbs/img2.jpg',  <-- thumb image path (this is only an example)
          img: 'assets/images/gallery/img2.jpg',           <-- image path (this is only an example)
          description: 'Image description 2'
        }
      ];

      openImageModal(imageSrc, images) {
        let imageModalPointer;
        for (let i = 0; i < images.length; i++) {
          if (imageSrc === images[i].img) {
            imageModalPointer = i;
            break;
          }
        }
        this.openModalWindow = true;
        this.images = images;
        this.imagePointer = imageModalPointer;
      }

      cancelImageModal() {
        this.openModalWindow = false;
      }
    }

    ```

3. In your component's template (file .html)

    ```
    <h3>Example - Default</h3>
    <br>
    <p> you can directly access "ImageModal" directive for both listing thumbnails and popup images</p>
    <br>
    <imageModal [modalImages]="images"></imageModal>

    <br><br>

    <h3>Example with thumbnail pointers</h3>
    <br>
    <p> you can list images in your file and then calling "ImageModal" directive to show images on popup only</p>
    <br>
    <div *ngFor="let img of images; let i= index">
    <div class="float-left" *ngIf="i <= 2" >
      <a class="more" *ngIf="i==2" (click)="openImageModal(img.img, images)"> +{{images.length - 3}} more </a>
      <img class="list-img" src="{{img.thumb}}"(click)="openImageModal(img.img, images)" alt='Image' />
    </div>
    </div>
    <div *ngIf="openModalWindow">
    <imageModal [modalImages]="images" [imagePointer] = "imagePointer" (cancelEvent) ="cancelImageModal()"></imageModal>
    </div>
    ```

However, I really recommend to try the official webpack example available in `demo/webpack`.


### Systemjs

1. In your system.config.js

    ```
    System.config({
      transpiler: 'typescript',
      typescriptOptions: {
        emitDecoratorMetadata: true,
        target: "ES5",
        module: "commonjs"
      },
      map: {
        '@angular'                          : 'node_modules/@angular',
        'rxjs'                              : 'node_modules/rxjs',
        'angular-modal-gallery'             : 'node_modules/angular-modal-gallery/bundles',  <-- add this here
        'app'                               : 'app'
      },
      paths: {
        'node_modules/@angular/*'           : 'node_modules/@angular/*/bundles'
      },
      meta: {
        '@angular/*'                        : {'format': 'cjs'}
      },
      packages: {
        'app'                               : {main: 'main', defaultExtension: 'ts'},
        'rxjs'                              : {main: 'Rx'},
        'angular-modal-gallery'             : {main: 'angular-modal-gallery.umd.js'},        <-- add this here
        '@angular/core'                     : {main: 'core.umd.min.js'},
        '@angular/common'                   : {main: 'common.umd.min.js'},
        '@angular/compiler'                 : {main: 'compiler.umd.min.js'},
        '@angular/platform-browser'         : {main: 'platform-browser.umd.min.js'},
        '@angular/platform-browser-dynamic' : {main: 'platform-browser-dynamic.umd.min.js'}
      }
    });
    ```

2. In index.html

    ```
    <html>

      <head>
        ...

        <!-- at the end, but before <script src="systemjs.config.js"></script> -->
        <!-- Required css to use this library -->
        <link rel="stylesheet" type="text/css" href="node_modules/angular-modal-gallery/styles/style.css">

        <script src="systemjs.config.js"></script>
        <script>
          System.import('app').catch(function (err) {
            console.error(err);
          });
        </script>
      </head>

      <body>
        <my-app>Loading...</my-app>
      </body>
    </html>
    ```

3. In yout main module add:

    ```
    import { ModalGalleryModule } from 'angular-modal-gallery';     <--- import this library

    @NgModule({
      declarations: [...],
      imports: [
        ...
        ModalGalleryModule.forRoot()    <--- add this in 'imports'
      ],
      bootstrap: [...],
    })
    export class AppModule {
    }
    ```


4. In you component (file .ts)

    ```
    import { Component } from '@angular/core';

    @Component({
      selector: 'my-app',
      styleUrls: ['./app/main.css'],
      templateUrl: 'app.html'
    })
    export class HomeComponent {

      openModalWindow: boolean = false;
      imagePointer: number;
      images = [                                <--- array with your images thumbs + img + description
        {
          thumb: 'assets/images/gallery/thumbs/img1.jpg',  <-- thumb image path (this is only an example)
          img: 'assets/images/gallery/img1.jpg',           <-- image path (this is only an example)
          description: 'Image description 1'
        },
        {
          thumb: 'assets/images/gallery/thumbs/img2.jpg',  <-- thumb image path (this is only an example)
          img: 'assets/images/gallery/img2.jpg',           <-- image path (this is only an example)
          description: 'Image description 2'
        }
      ];

      openImageModal(imageSrc, images) {
        let imageModalPointer;
        for (let i = 0; i < images.length; i++) {
          if (imageSrc === images[i].img) {
            imageModalPointer = i;
            break;
          }
        }
        this.openModalWindow = true;
        this.images = images;
        this.imagePointer = imageModalPointer;
      }

      cancelImageModal() {
        this.openModalWindow = false;
      }
    }

    ```

5. In your component's template (file .html)

    ```
    <h2> Example - Default</h2>
    <p> you can directly access "ImageModal" directive for both listing thumbnails and popup images</p>

    <imageModal [modalImages]="images"></imageModal>
    <h2>  Example with thumbnail pointers </h2>
    <p> you can list images in your file and then calling "ImageModal" directive to show images on popup only</p>
    <div *ngFor="let img of images; let i= index">
    <div class="float-left" *ngIf="i <= 2" >
      <a class="more" *ngIf="i==2" (click)="openImageModal(img.img, images)"> +{{images.length - 3}} more </a>
      <img class="list-img" src="{{img.thumb}}"(click)="openImageModal(img.img, images)" alt='Image' />
    </div>
    </div>
    <div *ngIf="openModalWindow">
    <imageModal [modalImages]="images" [imagePointer] = "imagePointer" (cancelEvent) ="cancelImageModal()"></imageModal>
    </div>
    ```

However, I really recommend to try the official systemjs example available in `demo/systemjs`.


## Examples

### Webpack (recommended)

check the official demo in `demo/webpack` and the live example [HERE](https://ks89.github.io/angular-modal-gallery/), compiled with `npm run build:github:aot`

To be able to run it follow these steps

1. `npm install` (from the root folder of this project)
2. `npm run clean:all`
3. `cd demo/webpack`
4. `npm install`
5. `cd ../..`
6. `npm run build`
7. `cd demo/webpack`
8. `npm start`

To build as dev in `dist` folder
- `npm run build:dev`

To build as prod without aot in `dist` folder
- `npm run build:prod`

To build as prod with aot in `dist` folder
- `npm run build:prod:aot` (if necessary re-follow these steps: 2,3,4,5,6,7,8)

To deploy this application on github (only for the owner of this repo)
- `npm run build:github:aot` and push `docs` folder (if necessary re-follow these steps: 2,3,4,5,6,7,8)


### Systemjs (not for production)

check the official demo in `demo/systemjs`

To be able to run it follow these steps

1. `npm install` (from the root folder of this project)
2. `npm run clean:all`
3. `cd demo/systemjs`
4. `npm install`
5. `cd ../..`
6. `npm run build`
7. `cd demo/systemjs`
8. `npm start`


### Angular-cli (COMING SOON)

TODO


## Contributing

check `CONTRIBUTING.md` in this repository


## Developer informations

check `DEVELOPER.md` in this repository


## License

The MIT License (MIT)

Copyright (c) 2017 Stefano Cappa (Ks89)<br>
Copyright (c) 2016 vimalavinisha

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
