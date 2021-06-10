# klinfys-register
Experiment kring kvalitetsregierliknande tillämpningar för klinisk fysiologi. Under ytan baserat på hälsoinfomatikstandarder som t.ex. openEHR och Snomed CT

I testfasenbörjar vi sannolikt med nedanstående verktyg 

## Tools:
* CKM - List/browse archetypes (and template examples): https://ckm.openehr.org/ckm/
* Archetype Designer - Create/edit templates (and if needed also create archetypes):https://tools.openehr.org/designer/
    * When importing or creating archeypes and templates with this tool connected to this reposotiry the files will likely end up in /local
    * ...then export OPT (Operational template) to be used in the next step. Please also save some useful example OPTs in the /opt subdirectory
* Better Studio - Create GUI/forms and queries, e.g. using https://tools.better.care/sandbox/studio/login Please also save some useful example forms (zip files) in the /forms subdirectory
* Snomed CT browser: https://browser.ihtsdotools.org/

## Example setup and settings

**Archetype designer**

Make sure you are logged in to Archetype designer using GitHub first:
![image](https://user-images.githubusercontent.com/1034001/121351630-477c3b00-c92c-11eb-92d2-b00a40d15a1e.png)

**Better Studio** (Sandbox)

* Make a (currently free) personal user registration at https://platform.better.care/sandbox (https://platform.better.care/sandbox/register)
* Await mail response with user/password
* Log in to the tool and select Administration or go to https://sandbox.better.care/studio/administration/domain-list
![image](https://user-images.githubusercontent.com/1034001/121383694-eeba9b80-c947-11eb-81dd-d4bc10bbba25.png)
* Set up a project (you can have several) in our case let's use the shared SFMI tutorial/course account at EHRscape as backend (Only fake patient data allowed - no clinical use!)
    * Project name: *whatever you like to call it* 
    * EHR Server URL: https://rest.ehrscape.com/
    * EHR Server username: SFMI
    * EHR Server password: *the password used in the course - see invitation email or ask colleague*

![image](https://user-images.githubusercontent.com/1034001/121383318-a69b7900-c947-11eb-8962-0ab98b3ea384.png)
