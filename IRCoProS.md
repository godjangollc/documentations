# Incremental Retro-Compatibility Programming Standard (IRCoProS o IRCPS)

**Author**: Esteban Chacon Martín
**Theme**: Code Standards and Retro-compatibility Management
**License**: MIT

In order to reduce considerably code breaks during new releases, even in the case we are using Unit Testing we present you with a programming standard that intends to avoid most and in some cases all of the code 
break. In this brief document we are going to use python programming as the example programming language.

1. Avoid in block codes or isolated blocks insertion of multiple or several lines.
For example in a code like below:

![image](https://github.com/user-attachments/assets/f4ec1af5-17dc-4adc-bfee-01b8e365e5e8)

Avoid extending the code by doing something like below (WRONG):

![image](https://github.com/user-attachments/assets/ca1c8073-f917-47bc-808a-9a37b8437cee)

And instead try a cleaner approach like below (CORRECT):

![image](https://github.com/user-attachments/assets/04335e4d-766c-448e-8fc9-5cb666024f00)

![image](https://github.com/user-attachments/assets/cc2d99cb-1798-42a7-8f6c-369384d0c490)

The above reduce PR complexity by focusing the problem in only one side, the new added files or extension methods 
`NOTE: You may be immediately thinking about the overmodularization problem, we are going to talk about it in another chapter. For now, let's just say it is not an issue for the first hundred commits unless you 
are making small, poor-quality commits little by little 😉`

2. Avoid removing or changing methods definitions, instead extend or override existing methods.
Always think in the people working with you, if you change a model definition both your code and his/her may break at some point because the methods don’t accept the same parameters any more.
`NOTE: If images are required here for clarity please tell me`
3. Use interfaces to force data compatibility through upgrades, never reduce interface signature, always extend it. 

In OOP we always use a base class containing all of the methods needed or attributes if is not an interface but instead an abstract class, in cases for languages like python that don’t enforce in compiling or startup time the classes’s signature you must raise the error on the constructor __new__ or the initializer __init__.

4. Use properties to replaces all attributes declarations and upgrade attributes functionalities, for example:

![image](https://github.com/user-attachments/assets/10f9209f-1f32-47e7-8628-a6e45d77f4cf)

In the above example we have a Cart that has a relation to the products by using the db, this is ok but maybe too much for a simple cart…. What if we want to use the session instead of the db for storing the 
cart for the current user? Then here comes properties to the rescue.

![image](https://github.com/user-attachments/assets/abec9203-ea34-495e-8221-530d26d87752)

The previous allows all already existing methods that uses the products from the db to transparently use now the session products, without breaking anything.

`NOTE: Maybe in the map we need to return directly product_pk as M2M fields could return a list of pk instead`

Another case example is when breaking down a variable into two variables for example, image into image_16x9 and image_landscape here we could use a property to return a default image url based on a cookie or 
simply just a default static to whichever aspect ratio you prefer. 

5. Once interfaces or execution stack becomes too large, consider marking them as deprecated and creating a new code under a different module with the vN (e.g. v1, v2, v3 …) naming convention at the start of the
new module.

Meaning once we shouldn’t extend any more the classes or the nested code in the functions we may need to think about migrating the code by following the process described above.

6.Don’t rush the deprecation, instead implement units tests for all of the new upgrades created that test compatibility for old deprecated code with existing non deprecated code.

7.For breaking changes you will also use the 5 step approach but this time without caring about retro-compatibility  (Is a breaking change after all) 
