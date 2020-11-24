This project hosts very simple but working geometries written in GDML and compatible with REST. The aim of those geometries is to serve as a reference to new REST users.

#### List of geometries available in this project

* **01.boxes**: Just two boxes shifted.

* **02.sphere**: A spherical volume contained in a stainlessteel vessel.


#### Visualizing the geometry using TGeoManager

The GDML geometries we write for REST are slightly modified, and are not directly compatible with ROOT. Our GDML files support the use of system variables `${VARIABLE}` and adding remote files inside a `http` entity.

We use a class named `TRestGDMLParser` to help reading this modified GDML file and translate it to be compatible with ROOT. The class responsible to read a GDML geometry in ROOT is a `TGeoManager`. See also the [TGeoManager documentation](https://root.cern/doc/v606/classTGeoManager.html) from ROOT pages.

Therefore, the `TRestGDMLParser` allows us to get a `TGeoManager` object that we can visualize in ROOT.

Still, we need to define few system variables, that in our case define the gas properties, and we hard-coded inside our simple geometries `GDML_XX` using `export` directive in bash shell terminal.

```
export GDML_GAS=Neon_ISO
export GDML_PRESSURE=1
export GDML_QUENCHER=0.02

restRoot
[0] TRestGDMLParser *g = new TRestGDMLParser();
[1] TGeoManager *geo = g->GetGeoManager();
[2] geo->GetTopVolume()->Draw("ogl");
```

#### Remote materials file

TOBE written


#### Geometry header and conection to TRestGeant4Metadata

-----

**⚠ WARNING: REST is under continous development.** This README is offered to you by the REST community. Your HELP is needed to keep this file up to date. You are very welcome to contribute fixing typos, updating information or adding new contributions. See also our [Contribution Guide](https://lfna.unizar.es/rest-development/REST_v2/-/blob/master/CONTRIBUTING.md).


