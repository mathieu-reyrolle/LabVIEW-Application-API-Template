# Application API Template Class
Classe template to wrap VI server based application control.

The project exposes two main LabVIEW classes :
- AppControlAPI.lvclass : this class will abstract the complexity of handling VI server references to call VIs in a LabVIEW executable
- TemplateAPI.lvclass : this is a template class that developper will duplicate to build the control wrapping class of a specific LabVIEW application (exe)

TemplateAPI.lvclass inherits from AppControlAPI.lvclass.


## Under the Hood...
Initialy developped with LabVIEW 2020 (maps), then saved for LabVIEW 2017 (maps -> variant attributes), it requieres LabVIEW 2018 in order to support malleable VIs on strictly defined VI types (connector). This type of data is not supported for malleable VIs by LabVIEW 2017.

### Malleable VI
The AppControlAPI exposes a (protected) malleable VI (Get Reference From Cache.vim) that handles VI reference openning and caching. Because it is malleable, its output reflect the connector type defined on the diagram that call this malleable VI. The Call by Reference node will 'auto-adapt' to this connector.

### Associative Array
A first draft of the parent class was originally build using Maps to cache the VI references. The cache was a map associating a variant (value) to a name (key). This had as drawback the need convert the retrieved variant from map (by name) to the expected strict VI definition type (connector) using Variant to data.

In the end, I replaced the map by a Variant and its attributes. It makes the code a little bit more simple, as the Get Attribute takes and return the expected strict VI definition type - without the need of 'casting' it explicitly.y

## Example application and wrapping class

A quick (and dirty) application, based on DQMH, is referenced by the project. It should allow you to build an executable with already defined *.ini file, and key allowing VI server access.

The ExampleAPI.lvclass class is a example class that allow to control the Example application. The application can be controlled when launched as source code (make sure to use an empty string as machine name parameter of Open Session.vi when targeting LabVIEW - and proper port), as an executable

I've made some tests (not extended - DISCLAIMER), it is also possible to control the application as an executable from a different bitness (exe build from LV20 64-bits vs. control class run from LV20 32-bits), and even as an executable build in different version of LabVIEW (exe build from LV19 vs control class run from LV20).

## LabVIEW version
Source code based on LabVIEW 2018 (malleable VIs must support strictly typed VI references)
