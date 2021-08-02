This project hosts very simple but working geometries written in GDML and compatible with REST. The aim of those geometries is to serve as a reference to new REST users.

#### List of geometries available in this project

* **01.boxes**: Just two boxes shifted.

* **02.sphere**: A spherical volume contained in a stainlessteel vessel.

These files are splitted in a main `setup.gdml` file that defines the main parameters, and a `geometry.xml` where the solids, volumes and physical volumes are defined.


#### Visualizing the geometry using TGeoManager

The GDML geometries we write for REST are slightly modified, and are not directly compatible with ROOT. Our GDML files support the use of system variables `${VARIABLE}` and adding remote `http` files and include them in a XML entity.

We use a class named `TRestGDMLParser` to help reading this modified GDML file and translate it to be compatible with ROOT. The class responsible to read a GDML geometry in ROOT is a `TGeoManager`. See also the [TGeoManager documentation](https://root.cern/doc/v606/classTGeoManager.html) from ROOT pages.

Therefore, the `TRestGDMLParser` allows us to get a `TGeoManager` object that we can visualize in ROOT.

Still, we need to define few system variables `GDML_XX`, that in our case define the gas properties, and that we hard-coded inside our simple geometry files. Using `export` directive in a bash shell terminal we will be able to give them a value before using the geometry.

```
export GDML_GAS=Neon_ISO
export GDML_PRESSURE=1
export GDML_QUENCHER=0.02

restRoot
[0] TRestGDMLParser *g = new TRestGDMLParser();
[1] TGeoManager *geo = g->GetGeoManager("setup.gdml");
[2] geo->GetTopVolume()->Draw("ogl");
```

#### Remote materials file

We use a common materials file that is located at the [Git materials project](https://lfna.unizar.es/rest-development/materials). The materials file is shared through the [following remote http location](https://sultan.unizar.es/materials/materials.xml) being the remote synchronized with the Gitlab project.

The materials file is included in the geometry using the following line:

```
<!ENTITY materials SYSTEM "http://sultan.unizar.es/materials/materials.xml">
```

We will be allowed to use any material included in that file for our geometrical volumes. If the material you need is not found we encourage to upload that material to the [Gitlab materials project](https://lfna.unizar.es/rest-development/materials) and share it with anyone else.

#### Geometry header and conection to TRestGeant4Metadata

When we launch `restG4`, the simulation conditions will be stored inside a `TRestGeant4Metadata` object at the output ROOT file. The GDML geometry defines the following header at the beginning of the `setup.gdml` file.

```
<!-- ##VERSION Boxes basic geometry Version 1.0 (${GDML_GAS} - ${GDML_PRESSURE}bar - Quencher: ${GDML_QUENCHER})## -->
```

That line will be stored at a data member inside the `TRestGeant4Metadata` class. Including the values we used for each of the `GDML_` variables. As it is seen at the following screen dump from `TRestGeant4Metadata::PrintMetadata`.

```
             ----------------------------------------------------------------------------------------------------              
              ||                                    Geant 4 version : 10.5.1                                    ||              
              ||                                     Random seed : 72146224                                     ||              
              ||     GDML geometry :  Boxes Basic geometry Version 1.1 (Neon_ISO - 1bar - Quencher: 0.02)     ||              
              ||                         GDML materials reference :  REST materials 1.1                         ||              
              ||                                    Max. Step size : 0.5 mm                                     ||              

```


-----

**⚠ WARNING: REST is under continous development.** This README is offered to you by the REST community. Your HELP is needed to keep this file up to date. You are very welcome to contribute fixing typos, updating information or adding new contributions. See also our [Contribution Guide](https://lfna.unizar.es/rest-development/REST_v2/-/blob/master/CONTRIBUTING.md).


