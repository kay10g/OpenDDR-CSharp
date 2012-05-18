OpenDDR-CSharp
==============
Dear OpenDDR user, 

this is the ALPHA version of C# porting of OpenDDR!

We have implemented all features of OpenDDR Java version (except for Group Aspect Identification) but we still need to improve performance.

All contributions are welcome.
Help OpenDDR grow up!


In the follow, the description of directory tree:
* __DDR-Simple-API__: contains porting project of W3C's Device Description Repository Simple API 
* __OpenDDR-CSharp__: contains porting project of Java version of OpenDDR
* __OpenDDRTest__: contains simple test project of C# version of OpenDDR


A basic explanation of the properties in oddr.properties:
* __oddr.ua.device.builder.path__: Path of the file that explain how to identify the devices. In this, for each builder, are specified the devices id that the builder handle and the identification rules
* __oddr.ua.device.datasource.path__: Path of the device datasource
* __oddr.ua.device.builder.patch.paths__: Path of the patch file for the builder file
* __oddr.ua.device.datasource.patch.paths__: Path of the patch file for the device data source
* __oddr.ua.browser.datasource.path__: Path of the browser data source
* __oddr.ua.operatingSystem.datasource.path__: Path of the operating system data source
* __ddr.vocabulary.core.path__: Path of the W3C vocabulary file
* __oddr.vocabulary.path__: Path of OpenDDR vocabulary
* __oddr.limited.vocabulary.path__: Path of the reduced vocabulary. This vocabulary is usefull to limitate the memory load. It can be safely left unspecified.
* __oddr.vocabulary.device__: IRI of the default vocabulary. It is the target namespace specified in a vocabulary
* __oddr.threshold__: Identification threshold. It is used to balance the request evaluation time and identification matching.

The sample class below shows how to use OpenDDR: 

<pre><code>
public class SimpleTest
{
	public static void Main(string[] args)
	{
		string oddrPropertiesPath = args[0];
		string userAgent = args[1];

		Properties props = new Properties(oddrPropertiesPath);

		Type stype = Type.GetType("Oddr.ODDRService, OpenDddr.Oddr");

		IService openDDRService = ServiceFactory.newService(stype, props.GetProperty("oddr.vocabulary.device"), props);

		IPropertyRef vendorRef = openDDRService.NewPropertyRef("vendor");
		IPropertyRef modelRef = openDDRService.NewPropertyRef("model");
		IPropertyRef displayWidthRef = openDDRService.NewPropertyRef("displayWidth");
		IPropertyRef displayHeightRef = openDDRService.NewPropertyRef("displayHeight");

		IPropertyRef[] propertyRefs = new IPropertyRef[] { vendorRef, modelRef, displayWidthRef, displayHeightRef };

		IEvidence e = new ODDRHTTPEvidence();
		e.Put("User-Agent", userAgent);

		IPropertyValues propertyValues = openDDRService.GetPropertyValues(e, propertyRefs);
		if (propertyValues.GetValue(vendorRef).Exists())
		{
			Console.WriteLine(propertyValues.GetValue(vendorRef).GetString());
		}

		if (propertyValues.GetValue(modelRef).Exists())
		{
			Console.WriteLine(propertyValues.GetValue(modelRef).GetString());
		}

		if (propertyValues.GetValue(displayWidthRef).Exists())
		{
			Console.WriteLine(propertyValues.GetValue(displayWidthRef).GetString());
		}

		if (propertyValues.GetValue(displayHeightRef).Exists())
		{
			Console.WriteLine(propertyValues.GetValue(displayHeightRef).GetString());
		}

		Console.ReadKey();
	}
}
</code></pre>