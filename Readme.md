# Foreword
The proposed code **Cloud BU** module is written in EO and implements some of the metrics of *JPeek*, a static code analysis tool.
Functionally, the module performs the tasks of calculating specific JPeek metrics utilizing the EO programming language and 
the capabilities of its standard object library.
Moreover, the model has the infrastructure of Java classes that integrates EO metrics into the general architecture of *JPeek*. 
This document focuses on the general architecture of the proposed module, which includes the following elements:
1.  A subroutine of parsing and reconstructing data of a class being assessed in the form of EO objects.
2.	A class that integrates EO metrics into the general pipeline of the application, providing input and formatting output for each metric run.
3.	A technique of embedding the EO programming language transpiler into the building process of *JPeek*.

# Integration of EO metrics into the pipeline of JPeek
To isolate EO metrics from XML, the following techniques are applied:
1.	First, the “Skeleton.xml” document containing information on the structure details of the class being assessed is parsed and reconstructed 
in the form of EO objects describing the structure of the class. This stage is done by the [“EOSkeleton”](https://github.com/HSE-Eolang/jpeek/blob/master/src/main/java/org/jpeek/skeleton/eo/EOSkeleton.java) class.
To call and use EO objects, the *“EOSkeleton”* class utilizes a basic late binding technique made possible through specific embedding of the EO transpiler 
into the building process described in the following section.
2.	Second, the [EOCalculus](https://github.com/HSE-Eolang/jpeek/blob/master/src/main/java/org/jpeek/calculus/eo/EOCalculus.java) class extracts the “class” EO object from the formed “EOSkeleton” instance and loads 
it into the metric written in EO.To call EO objects of metrics, the *“EOCalculus” class* utilizes a basic late binding technique made possible 
through specific embedding of the EO transpiler into the building process described in the following section. The output of the metric is put 
into the XML file in the standard way to embed the results to the general pipeline of the *JPeek* application.

# Embedding the EO transpiler into the building process
Since the EO to Java [transpiler](https://github.com/HSE-Eolang/eo) enhanced by the Team is distributed as a Maven artifact it was embedded into 
the building process through the following steps:
1.	The artifact *“org.eolang.eo-runtime”* was added as a runtime dependency to the “pom.xml” of the JPeek project. It was needed to access 
the standard object library of EO at runtime.
2.	The artifact *“org.eolang.eo-maven-plugin”* was added as a plugin as the first step of the building process. The plugin is instructed to transpile 
all the EO metrics before building Java classes of the rest of the JPeek project structure. This makes possible to implement late binding of *Java2EO*
references in Java code and, hence, to introduce a basic variant of *Java2EO* interoperability.
