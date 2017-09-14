# FME® ListKeyValuePairExtractor
## About
The _ListKeyValuePairExtractor_ creates new attributes based on a list of keys (attribute names) and a list of values.   
For example, a feature might contain the following list elements:

_list{0}.name = 'StringData'_  
_list{0}.data = 'Test Value'_   
_list{1}.name = 'IntData'_  
_list{1}.data = 32536_   
_list{2}.name = 'FloatData'_  
_list{2}.data = 23.67_

If the user sets the _Attribute Name List_ to _list{**n**}.name_ and the _Attribute Value List_ to _list{**n**}.data_, the transformer will add  the following attributes to the output feature:

_StringData = 'Test Value'_  
_IntData = 32536_  
_FloatData = 23.67_

### IMPORTANT
In the example above, you can fill in any integer for list element **n**. Due to the fact that FME does not support  nested lists as an input parameter for custom transformers, you will actually have to specify it as a list attribute, using FME's List Element Selection dialog.  
This means you can select regular attributes for the _Attribute Name List_ and _Attribute Value List_ parameters, but that will result in an error. It also means that if you have a basic unnested list, the index you specify does not matter. The _ListKeyValuePairExtractor_ will simply fetch the complete list.

For example, if you want to use all the values in _list{}.name_ as your _Attribute Name List_, you could set the input parameter to _list{**0**}.name_. If there are 3 elements in this list, the transformer will fetch _list{0}.name_, _list{1}.name_ and _list{2}.name_. But this will also be the case if you set it to _list{**2**}.name_, for example.

However, if you want to use all the values of _list{}.nestedlist{}_, the index of the parent _list{}_ matters and should exist! For example, if you set the _Attribute Name List_ parameter to _list{**3**}.nestedlist{0}_, only the _nestedlist{}_ elements within _list{**3**}_ will be used.  

Please note that the created attributes will not be exposed. You can use an [AttributeExposer](https://www.safe.com/transformers/attribute-exposer/) afterwards to achieve that.

###  Notes
- Also available on [FME Hub](https://hub.safe.com/transformers/listkeyvaluepairextractor) for convenient installation.  
- This transformer has been tested on Python 2.7 and 3.4.  
- Released under [GNU General Public License v3.0](https://github.com/SanderSchaminee/fme-listkeyvaluepairextractor/blob/master/LICENSE).  
- If you notice a bug or desire a new feature, please contact me. Or make a pull request!  
- The [test workspace](https://github.com/SanderSchaminee/fme-listkeyvaluepairextractor/blob/master/ListKeyValuePairExtractorTest.fmw) is used for testing and provides some examples.

## Usage
**Attribute Name List**  
Specify the list attribute that should be used to extract the attribute names (keys). 
The list can have missing elements (in which case no attribute will be written), but the list should be in the same order as the _Attribute Value List_. Please read the overview section of this documentation for more information.

**Attribute Value List**  
Specify the list attribute that should be used to extract the attribute values. 
If a value is missing or null, a warning will be shown in the log. The value list does not need to be of the same size as the _Attribute Name List_, but if the created pairs should match, the list has to be in the same order. Please read the overview section of this documentation for more information.

**Conflict Resolution**  
If one or more of the _Attribute Name List_ elements already exist as a feature attribute, setting this value to "Keep Original Attributes" will prevent the attribute values from being overwritten by the _Attribute Value List_ elements.
