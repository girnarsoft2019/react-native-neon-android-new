# react-native-neon-android
A React Native package to take photos using custom camera or custom gallery in android.
- (Supports on react-native >= 0.47.0)
## Installation
```
npm install react-native-neon-android
```
## Linking
```
react-native link
```
## Manual linking
1. Add the following lines to `android/settings.gradle`:
```ruby
include ':react-native-neon-android'
project(':react-native-neon-android').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-neon-android/android/app')
```
2. Add the compile line to the dependencies in `android/app/build.gradle`:
```ruby
dependencies {
    ---------------------
    compile project(':react-native-neon-android')
}
```
3. Add the following line to `android/app/src/main/java/MainApplication.java`:
```ruby
@Override
protected List<ReactPackage> getPackages() {
   return Arrays.<ReactPackage>asList(
       new MainReactPackage(),
         new NeonReactPackage()
   );
}
```
## Add the following lines to `android/app/src/AndroidManifest.xml`:
```ruby
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:tools="http://schemas.android.com/tools"
          --------------------------------------------->
  <application
     ---------------------
     tools:replace="android:theme,android:allowBackup">
  </application>
   
</manifest>   
```
## Add the following lines to `android/app/build.gradle`:
```ruby
android {
    ----------------------
    dataBinding {
        enabled = true
    }
}
```
## Usage
### Import NeonAndroid
```ruby
import NeonAndroid from 'react-native-neon-android';
```
### React methods
1. ``` NeonAndroid.collectPhotos(action, neonParamsJson, callback) ```
2. ``` NeonAndroid.livePhotos(neonParamsJson, callback) ```

where:
- action (NeonAndroid.OPEN_NEUTRAL,NeonAndroid.OPEN_CAMERA or NeonAndroid.OPEN_GALLERY)
   - NeonAndroid.OPEN_NEUTRAL - for Neutral
   - NeonAndroid.OPEN_CAMERA - for Camera
   - NeonAndroid.OPEN_GALLERY - for Gallery
- neonParamsJson - Json of neonParams object(can be pass as **null**)
- callback - will return Json of array of fileInfo

Example:
```
NeonAndroid.collectPhotos(NeonAndroid.OPEN_NEUTRAL, JSON.stringify(neonParams), (fileInfoListJson) => console.log(fileInfoListJson));
```
where fileInfoListJson is the json of array of fileInfo which you will get in callback, you can convert it into object using JSON.parse(fileInfoListJson).

### neonParams
```
{
    libraryMode: 0,
    numberOfPhotos: 0,
    cameraFacing: 0,
    cameraOrientation: 0,
    cameraViewType: 0,
    galleryViewType: 0,
    tagEnabled: false,
    flashEnabled: true,
    cameraSwitchingEnabled: false,
    cameraToGallerySwitchEnabled: false,
    enableFolderStructure: true,
    galleryToCameraSwitchEnabled: false,
    isRestrictedExtensionJpgPngEnabled: true,
    enableImageEditing: false,
    locationRestrictive: false,
    hideCameraButtonInNeutral: false,
    hideGalleryButtonInNeutral: false,
    hasOnlyProfileTag: false,
    profileTagName: null,
    imageTagListJson: null,
    alreadyAddedImagesJson: null,
};
```
- Detail:

| Key | Type | Default Value | Required | Description |
| --- | --- | --- | --- | --- |
| libraryMode | int | 0 | no | 0-Relax, 1-Restrict(a pop-up will be shown on before exit from library) |
| numberOfPhotos | int | 0 | no | Pass 0 for no restriction, if any number other than 0 is passed then only that particular number of images are allowed  |
| cameraFacing | int | 0 | no | 0-Rear, 1-Front |
| cameraOrientation | int | 0 | no | 0-Landscape, 1-Portrait |
| cameraViewType | int | 0 | no | 0-Normal camera, 1-Gallery preview camera |
| galleryViewType | int | 0 | no | 0-Grid Structure, 1-Horizontal Structure |
| tagEnabled | boolean | false | no* | if passes true then **imageTagListJson** must not be null or empty |
| flashEnabled | boolean | true | no | If passes true then flash is enabled |
| cameraSwitchingEnabled | boolean | false | no | If passes true then camera swithing is allowed |
| cameraToGallerySwitchEnabled | boolean | false | no | If passes true then access to gallery from camera screen is allowed |
| enableFolderStructure | boolean | true | no | If passes true then folder structure for gallery is enabled |
| galleryToCameraSwitchEnabled | boolean | false | no |  If passes true then access to camera from galeery screen is allowed  |
| isRestrictedExtensionJpgPngEnabled | boolean | true | no |  If passes true then only jpg and png format are allowed |
| enableImageEditing | boolean | false | no |  |
| locationRestrictive | boolean | false | no | If passes true then GPS location is required |
| hideCameraButtonInNeutral | boolean | false | no |  If passes true then camera button will hide on neutral screen |
| hideGalleryButtonInNeutral | boolean | false | no | If passes true then gallery button will hide on neutral screen |
| hasOnlyProfileTag | boolean | false | no* | if passes true then **profileTagName** must not be null or empty |
| profileTagName | String | null | no |  |
| imageTagListJson | Json | null | no | Json of array of **imageTag** |
| alreadyAddedImagesJson | Json | null | no | Json of array of already added images(**fileInfo**) |

- fileInfo 
```
{
    dateTimeTaken: "1522229825513",
    fileCount: 0,
    filePath: "https://images.unsplash.com/photo-1441742917377-57f78ee0e582?h=1024",
    selected: false,
    source: "PHONE_GALLERY",
    type: "IMAGE",
    latitude: "",
    longitude: "",
    fileName: "",
    displayName: "",
    fileTag: {
        mandatory: false,
        numberOfPhotos: 1,
        tagId: "3",
        tagName: "Tag3",
        location: null,
    }
};
```
Details of fileInfo:

| Key | Type | Description|
| --- | --- | --- |
| dateTimeTaken | String | dateTime in milliseconds|
| fileCount | int | | 
| filePath | String | local path or url |
| selected | boolean | |
| source | boolean| |
| type | String | |
| latitude | String | |
| longitude | String | |
| fileName | String | |
| displayName | String | |
| fileTag | object | **imageTag**|

- imageTag
```
{
    mandatory: false,
    numberOfPhotos: 1,
    tagId: "1",
    tagName: "Tag1",
    location: null,
};
```
Details of imageTag:

| Key | Type | Description|
| --- | --- | --- |
| mandatory | boolean | |
| numberOfPhotos | int | number of images required for particular tag | 
| tagId | String | |
| tagName | String | |
| location | object | Location used in android |

## Usage Example
- Example Project [GitHub Link](https://github.com/rajeev15june/neon-module-example).
```ruby
import React, {Component} from 'react';
import {
    StyleSheet,
    Text,
    View,
    TouchableNativeFeedback, FlatList, Image, ScrollView
} from 'react-native';
import NeonAndroid from 'react-native-neon-android';
import * as Constants from './Constants';

let imageTagList = [
    {
        mandatory: false,
        numberOfPhotos: 1,
        tagId: "1",
        tagName: "Tag1",
        location: null,
    },

    {
        mandatory: false,
        numberOfPhotos: 1,
        tagId: "2",
        tagName: "Tag2",
        location: null,
    }

];

let neonParams = {
    libraryMode: 0,
    numberOfPhotos: 0,
    cameraFacing: 0,
    cameraOrientation: 0,
    cameraViewType: 0,
    galleryViewType: 0,
    tagEnabled: false,
    flashEnabled: true,
    cameraSwitchingEnabled: false,
    cameraToGallerySwitchEnabled: false,
    enableFolderStructure: true,
    galleryToCameraSwitchEnabled: false,
    isRestrictedExtensionJpgPngEnabled: true,
    enableImageEditing: false,
    locationRestrictive: false,
    hideCameraButtonInNeutral: false,
    hideGalleryButtonInNeutral: false,
    hasOnlyProfileTag: false,
    profileTagName: null,
    imageTagListJson: null,
    alreadyAddedImagesJson: null,
};


export default class App extends Component<Props> {
    constructor() {
        super();
        this.openNeon = this.openNeon.bind(this);
        this.prepareList = this.prepareList.bind(this);
        this.state = {
            images: [],
            alreadyAddedImages: []
        }

    }

    openNeon(position) {
        let params = neonParams;
        switch (position) {
            case Constants.OPEN_NEUTRAL:
                params.profileTagName = 'Cover';
                params.hasOnlyProfileTag = true;
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_NEUTRAL, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_CAMERA:
                params.cameraOrientation = 1;
                params.flashEnabled = true;
                params.cameraSwitchingEnabled = true;
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_CAMERA, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_CAMERA_PRIORITY:
                params.cameraOrientation = 1;
                params.flashEnabled = true;
                params.cameraSwitchingEnabled = true;
                params.tagEnabled = true;
                params.libraryMode = 1;
                params.numberOfPhotos = 2;
                params.cameraToGallerySwitchEnabled = true;
                params.imageTagListJson = JSON.stringify(imageTagList);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_CAMERA, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_GALLERY_FOLDERS:
                params.galleryToCameraSwitchEnabled = false;
                params.numberOfPhotos = 0;
                params.enableFolderStructure = true;
                params.imageTagListJson = JSON.stringify(imageTagList);
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_GALLERY_FOLDERS_PRIORITY:
                params.galleryToCameraSwitchEnabled = true;
                params.numberOfPhotos = 0;
                params.enableFolderStructure = true;
                params.imageTagListJson = JSON.stringify(imageTagList);
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_GALLERY_FILES:
                params.galleryToCameraSwitchEnabled = false;
                params.numberOfPhotos = 2;
                params.enableFolderStructure = false;
                params.libraryMode = 1;
                params.galleryViewType = 0;
                params.tagEnabled = true;
                params.imageTagListJson = JSON.stringify(imageTagList);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_GALLERY_FILES_PRIORITY:
                params.galleryToCameraSwitchEnabled = true;
                params.numberOfPhotos = 0;
                params.galleryViewType = 0;
                params.enableFolderStructure = false;
                params.imageTagListJson = JSON.stringify(imageTagList);
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;

            case Constants.OPEN_GALLERY_HORIZONTAL:
                params.galleryToCameraSwitchEnabled = false;
                params.numberOfPhotos = 0;
                params.galleryViewType = 1;
                params.enableFolderStructure = false;
                params.imageTagListJson = JSON.stringify(imageTagList);
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_GALLERY_HORIZONTAL_PRIORITY:
                params.galleryToCameraSwitchEnabled = true;
                params.numberOfPhotos = 0;
                params.galleryViewType = 1;
                params.enableFolderStructure = false;
                params.imageTagListJson = JSON.stringify(imageTagList);
                params.alreadyAddedImagesJson = JSON.stringify(this.state.alreadyAddedImages);
                NeonAndroid.collectPhotos(NeonAndroid.OPEN_GALLERY, JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;
            case Constants.OPEN_LIVE_PHOTOS:
                NeonAndroid.livePhotos(JSON.stringify(params), (imageCollectionJson) => this.prepareList(imageCollectionJson));
                break;

        }
    }

    prepareList(imageCollectionJson) {
        if (JSON.parse(imageCollectionJson).length === 0) {
            this.setState({alreadyAddedImages: []});
            return;
        }
        let data = JSON.parse(imageCollectionJson);
        this.setState({
            alreadyAddedImages: data,
            images: data
        });
    }

    renderItem({item, index}) {
        let imageUrl = '';
        if (item.filePath.toString().contains("//")) {
            imageUrl = item.filePath;
        } else {
            imageUrl = "file://" + item.filePath;
        }
        return (
            <View>
                <Image
                    style={{
                        alignSelf: 'center',
                        margin: 5,
                        width: 110,
                        height: 100,
                    }}
                    source={{uri: imageUrl}}
                />
            </View>

        )
    }


    render() {
        return (
            <ScrollView>
                <View style={styles.container}>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_NEUTRAL)}>
                        <Text style={styles.instructions}>
                            Neutral
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_CAMERA)}>
                        <Text style={styles.instructions}>
                            Camera
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_CAMERA_PRIORITY)}>
                        <Text style={styles.instructions}>
                            Camera Priority
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_FOLDERS)}>
                        <Text style={styles.instructions}>
                            Gallery(Folders)
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_FOLDERS_PRIORITY)}>
                        <Text style={styles.instructions}>
                            Gallery Priority(Folders)
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_FILES)}>
                        <Text style={styles.instructions}>
                            Gallery(Files)
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_FILES_PRIORITY)}>
                        <Text style={styles.instructions}>
                            Gallery Priority(Files)
                        </Text>
                    </TouchableNativeFeedback>

                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_HORIZONTAL)}>
                        <Text style={styles.instructions}>
                            Gallery(Horizontal)
                        </Text>
                    </TouchableNativeFeedback>
                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_GALLERY_HORIZONTAL_PRIORITY)}>
                        <Text style={styles.instructions}>
                            Gallery Priority(Horizontal)
                        </Text>
                    </TouchableNativeFeedback>

                    <TouchableNativeFeedback onPress={() => this.openNeon(Constants.OPEN_LIVE_PHOTOS)}>
                        <Text style={styles.instructions}>
                            Live Photos
                        </Text>
                    </TouchableNativeFeedback>

                    <FlatList
                        contentContainerStyle={styles.list}
                        data={this.state.images}
                        renderItem={this.renderItem}/>
                </View>
            </ScrollView>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#F5FCFF',
    },
    welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    instructions: {
        textAlign: 'center',
        alignSelf: 'stretch',
        backgroundColor: 'green',
        padding: 12,
        color: 'white',
        fontWeight: 'bold',
        fontSize: 16,
        marginTop: 10,
    },
    list: {
        justifyContent: 'center',
        flexDirection: 'row',
        flexWrap: 'wrap'
    }
});

let fileInfo = {
    dateTimeTaken: "1522229825513",
    fileCount: 0,
    filePath: "https://images.unsplash.com/photo-1441742917377-57f78ee0e582?h=1024",
    selected: false,
    source: "PHONE_GALLERY",
    type: "IMAGE",
    latitude: "",
    longitude: "",
    fileName: "",
    displayName: "",
    fileTag: {
        mandatory: false,
        numberOfPhotos: 1,
        tagId: "3",
        tagName: "Tag3",
        location: null,
    }
};
```

## Author:
- **Rajeev Kumar**(*rajeev15june@gmail.com*)
