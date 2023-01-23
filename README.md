# Discovering-The-Poet-with-ARCore-Challenge
This repository as part of the GEOspatial API Challenge using ARCore.
## Inspiration

**Félix Rubén García Sarmiento**, known as **Rubén Darío (Metapa, January 18, 1867-León, February 6, 1916)**, was a poet, writer, journalist, and maximum representative of literary modernism in the Spanish language. He is perhaps the poet who has had the greatest and most lasting influence on 20th-century poetry in the Hispanic sphere, and for this reason, he is called the **"prince of Castilian letters"**.

His writings and legacy are still being studied in different parts of the world. The literature that he left us still serves as an inspiration for the new generations of writers and poets who currently, through his art, express their vision of the modern world from a more human perspective.

## What it does

Visitors, through an apk previously installed on their phones, will be able to view some 3D elements that are related to the elements that Ruben Dario used in his literary writings. The user will be able to walk near the monument and will be able to discover objects related to life and history. In such a way that it manages to arouse interest in studying and valuing the contribution that Ruben Dario made in different areas.

## How we built it

## Configuration of the Project

1. In unity, configure the project so that it can run on android devices.
2. Within **Project Settings**, in the **XR Plug-in Management** section, activate the option that says **ARCore**
3. Within **Project Setting**, in the section that says **Player** we go to the option that says **Other Setting** and remove **Vulkan** from **Graphics APIs**
4. Where it says **Minimum API level** we choose a more recent android version. In my case, the project was made using API Level 24
5. In the part that says **Scripting Backend** we change from **mono** to **IL2CPP** and in **Api Compatibility Level** we select the option that says **.Net 4.x**
6. In the part that says **Target Architectures** we select the option that says **ARM64**

## Download and Installation of ARCore Extensions Package into Unity Package Manager

7. Then we go to the Unity menu and where it says **Windows** we look for the option that says **Package Manager**. In the **+** icon we choose the option that says **Add package from github url...** and paste the following **https://github.com/google-ar/arcore-unity-extensions. git**. This step allows us to install the ARCore Extensions package.

## Activation of our ARCore API on GCP

8. Then we go to our GCP console. We can create a new project or work on one that we have already created. We need to activate our ARCore API. To do this, we write the word ARCore in the search engine. Then we proceed to click on the **activate** button and it will generate **API-KEY**
9. Copy the **API-KEY**, return to Unity and in **Project Setting** in the **XR Plug-in Management** section select **ARCore Extensions** and paste the API-KEY where it says **Android API KEY**. We also have to select the option that says **Geospatial**

## Incorporating the elements of ARCore into our scene in Unity.

Being inside Unity, we locate ourselves where the **Hierarchy** section is, we right click, select **XR** and we will add the following elements:
**_AR Session_**
**_AR Session Origin_**
**_ARCore Extensions_**
And we put an object of type **"empty"** to which we will put the name of **_VPSManager_**

## Part of the code inside the generated script to work with ARCore and Unity

Added ARCore Fundamentals 

```
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using Google.XR.ARCoreExtensions;

```
We declare a public-type structure to register three values which would be the latitude, longitude, and altitude to be able to position our 3D object

```
[Serializable] public struct EarthPosition
    {
        public double Latitude;
        public double Longitude;
        public double Altitude;
    }

```

We create a function to validate that our application can be installed on the smart device.

```
 private void VerifyGeospatialSupport()
    {
        var result = earthManager.IsGeospatialModeSupported(GeospatialMode.Enabled);
        switch (result)
        {
            case FeatureSupported.Supported:
                Debug.Log("Ready to use VPS");
                PlaceObjects();
                break;
            case FeatureSupported.Unknown:
                Debug.Log("Unknown device...");
                Invoke("VerifyGeospatialSupport", 5.0f);
                break;
            case FeatureSupported.Unsupported:
                Debug.Log("VPS Unsupported, sorry about that");
                break;
        }
    }

```

A private function is created to control the position of the 3D object taking latitude, longitude and altitude as parameters.

```
if (earthManager.EarthTrackingState == TrackingState.Tracking)
        {
            var geospatialPosse = earthManager.CameraGeospatialPose;

            foreach (var obj in geospatialObjects)
            {
                var earthPosition = obj.EarthPosition;
                var objAnchor = ARAnchorManagerExtensions.AddAnchor(aRAnchorManager, earthPosition.Latitude, earthPosition.Longitude, earthPosition.Altitude, Quaternion.identity);
                Instantiate(obj.ObjectPrefab, objAnchor.transform);

            }
        }
```

## Challenges we ran into
1. Register coordinate points in terms of Longitude and Latitude to position 3D objects.

2. Export the Textures within Unity

3. Adjust the size of 3D models within Unity

4. Install ARCore on smartphones for testing as not all smart devices can support the app.

5. Perform on-site tests.

## Accomplishments that we're proud of

1. Build an apk file that can be installed on smart devices that works with geospatial position and GCP API to have a life experience through Augmented Reality.

2. Create a project prototype that can be applied to different environments in different parts of the world.

3. Propose a new proposal for interaction between real-world events and be able to bring them to life through Virtual Reality using ARCore

## What we learned

1. Download and Install the ARCore package from **Git Hub** and place it inside Unity via the package manager.

2. Learn about the main fundamentals of ARCore that are needed to create an Augmented Reality project that uses geospatial parameters.

3. Put Unity in the development environment to generate an **apk** that can work with the fundamental elements of ARCore

4. Import 3D objects in **.fbx** format into Unity.

5. Import the textures of 3D objects into Unity.

6. Integrate Unity with Blender to generate a pipeline of work

7. Connect the 3D objects with the ARCore scripts using the Latitude and Longitude points previously taken using google maps.

8. Create the final file to be transferred to the smart devices to proceed with the installation of the **apk** generated from Unity.

9. Activate the ARCore API within GCP to generate the key and incorporate it within the Unity project

10. Conversion of coordinates of Latitude and Longitude to the Gravitational Model of the earth **EGM96**, this is in order to determine at what height we must locate the 3D model so that when it is used within the application it can be displayed within the smart device

## What's next for Discovering the Poet

Improve the functionalities of the script in order to have a better performance of the created algorithm.
Enhance latitude, longitude, and height points to more accurately position 3D objects
Add 3D objects that contain some kind of animation
Add some descriptive text to provide a better user experience
Perform tests on various smartphone models to determine if the app can be installed
Publish the apk in the google play store so that all visitors to the monument can try the application
Visit the Ruben Dario house museum to create an Augmented Reality prototype with ARCore where users can learn more about this character studied worldwide.


##The Complete Script used for this project
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using Google.XR.ARCoreExtensions;
using System;

public class VPSManager : MonoBehaviour
{
    [SerializeField] private AREarthManager earthManager;

    [Serializable] public struct GeospatialObject
    {
        public GameObject ObjectPrefab;
        public EarthPosition EarthPosition;
    }

    [Serializable] public struct EarthPosition
    {
        public double Latitude;
        public double Longitude;
        public double Altitude;
    }

    [SerializeField] private ARAnchorManager aRAnchorManager;
    
    [SerializeField] private List<GeospatialObject> geospatialObjects = new List<GeospatialObject>();

    // Start is called before the first frame update
    void Start()
    {
        VerifyGeospatialSupport();
    }

    private void VerifyGeospatialSupport()
    {
        var result = earthManager.IsGeospatialModeSupported(GeospatialMode.Enabled);
        switch (result)
        {
            case FeatureSupported.Supported:
                Debug.Log("Ready to use VPS");
                PlaceObjects();
                break;
            case FeatureSupported.Unknown:
                Debug.Log("Unknown device...");
                Invoke("VerifyGeospatialSupport", 5.0f);
                break;
            case FeatureSupported.Unsupported:
                Debug.Log("VPS Unsupported, sorry about that");
                break;
        }
    }

    private void PlaceObjects()
    {
        if (earthManager.EarthTrackingState == TrackingState.Tracking)
        {
            var geospatialPosse = earthManager.CameraGeospatialPose;

            foreach (var obj in geospatialObjects)
            {
                var earthPosition = obj.EarthPosition;
                var objAnchor = ARAnchorManagerExtensions.AddAnchor(aRAnchorManager, earthPosition.Latitude, earthPosition.Longitude, earthPosition.Altitude, Quaternion.identity);
                Instantiate(obj.ObjectPrefab, objAnchor.transform);

            }
        }

        else if (earthManager.EarthTrackingState == TrackingState.None)
        {

            Invoke("PlaceObjects", 5.0f);

        }
    }



}
```
