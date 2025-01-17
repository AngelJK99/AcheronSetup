# Acheron Project Keyboard Creator

A tool to create a KiCAD keyboard project using the Acheron Libraries.

This keyboard creator tool also encloses the tolerances and rules used for the Acheron Project in the keyboard PCB projects, uniformizing their design.

## Introduction

The Acheron Keyboard Creator tool is a bash-script tool aimed at automating the process of creating a KiCAD PCB project for a keyboard PCB. The produced files are ready-to-use and can be edited and modified using the latest KiCAD nightly (november 4, 2021 or newer) and include configuration settings such as copper clearance and tolerance, soldermask clearance and minimum width aimed at being compatible across multiple factories. This script has been developed and tested in bash. It also functions in git bash; no other platform or terminal was tested.


The joker templates are documented [here](http://acheronproject.com/multimcu_article/multimcu_article/). **BE SURE TO READ THIS DOCUMENTATION**. These templates will contain a microcontroller footprint with ancillary components that make the resulting PCB compatible with a myriad of microcontrollers.

## Usage

Execute ```./keybaord\_create.sh \[options\] \[arguments\]```

This will, in this order,

- Initialize the root folder as a git repository and change the current repository name to `main`;
- Create a folder ```kicad_files``` with:
    - A KiCAD project file ```project.kicad_pro```
    - A KiCAD PCB layout file ```project.kicad_pcb```
    - A KiCAD schematic file ```project.kicad_sch```
    - A KiCAD libraries backup file ```project.kicad_prl```
    - A symbol library table ```sym-lib-table```
    - A footprint library table ```fp-lib-table```
- Create a libraries folder ```./kicad_files/libraries``` folder where the following libraries will be added. Each library is automatically added to the respective library tables:
    - acheron_Symbols
    - acheron_Components.pretty
    - acheron_Connectors.pretty
    - acheron_Hardware.pretty
    - acheron_MX.pretty **(*)**
    - acheron_Graphics.pretty
    - acheron_Logos.pretty

In the end the ```./kicad_files``` folder will contain a fully working KiCAD project with ready to use added libraries.

## Arguments

These are the required arguments for the script to execute. Defaults are found in parentheses after each

- ```[-t, --template]``` Choose what template to use. ('BLANK')
    - ```BLANK``` f or a blank PCB with pre-configured settings
    - ```J48``` for the 48-pin joker template
    - ```J64``` for teh 64-pin joker template
- ```[-p, --projectname]``` Sets the names of the kicad project files ('project')
- ```[-kd, --kicaddir]``` Choose the project parent folder name ('kicad_files')
- ```[-ld, --libdir]``` Choose the folder inside KICADDIR where libraries and submodules are added. ('libraries')
- ```[-s, --switchtype]``` Select what switch type library submodule to be added. ('MX')
    - ```MX``` for [simple MX support](https://github.com/AcheronProject/acheron_MX.pretty)
    - ```MX_soldermask``` for [MX support with covered front switches](https://github.com/AcheronProject/acheron_MX_soldermask.pretty)
    - ```MXA``` for [MX and Alps suport](https://github.com/AcheronProject/acheron_MXA.pretty)
    - ```MXH``` for [MX hostwap](https://github.com/AcheronProject/acheron_MXH.pretty)

## Options

Additionally, the following optional flags are available.

- ```[-h, --help]``` Displays a help message and exits.
- ```[-v, --verbose]``` Enable verbose logging.
- ```[-pc, --purgeclean]``` Deletes all generated files before execution (*.git folders and files and the KICADDIR), leaving only the original repository, and proceeds normal execution.  WARNING: deletions are definitive!
- ```[-cc, --cleancreate]```  Creates cleanly, removing all base files including this script, leaving only the final files.  WARNING: deletions are definitive!
- ```[-ng, --nographics]``` Do not include graphics library submodule.
- ```[-nl, --nologos]``` Do not include logos library submodule.
- ```[-n3, --no3d]``` Do not include 3D models library submodule.
- ```[-nr, --norepo]```  Do not init a git repository.
- ```[-ns, --nosubmodule]``` Do not add libraries as git submodules. (Note: if the --norepo flag is not passed, a git repository will still be initiated). 

## Design notes

The PCB files generated contain a myriad of information regarding tolerances, usage and copper pours. These informations are replicated here.

### Usage comments

This PCB file was generated through the Acheron Setup script, an automated tool to generate design-ready KiCAD schematic and PCB files for keyboard projects; it is offered under a no-liability, as-is clause. Please visit http://github.com/AcheronProject/AcheronSetup for more information.

The files generated are compatible with KiCAD developmental ("nightly") versions up to august 16, 2021 version. These files should also be accompanied of a librires folder where the used libraries are added as submodules which last commit should point to the remote's HEAD; please note that the files need such libraries to fully work.

For requests/issues please submit a issue in the github folder. Do not attempt to contact the developers directly. We ask that bugs/problems be reported through the issues page too.

### Notes on tolerances

The manufacturing tolerances used for PCBs in the Acheron Project are a way to uniformize the design settings of the projects. These settings were obtained using mainly three manufacturers: PCBWay, Elecrow and JLCPCB (see their capabilities pages on references [1-3]). These are big players in the asian PCB manufacturing market and the tolerances practiced by them are accepted industry-wide; hence, the values used here should be enough for most manufacturers.

It must also be kept in mind that, since the Acheron Project is heavily DIY-oriented, the tolerance settings used were the easiest or cheapest available. For instance, Elecrow offers "standard PCB" and "advanced PCB" services ([2]), while PCBWay offers "normal difficulty", "medium difficulty" and "high difficulty" processes ([3]). In both manufacturers, the advanced settings have smaller tolerances and better manufacturing features but are significantly more expensive, specially when paired with PCB assembly services. Such settings are meant for more complex projects (high-speed or analog signal processing, finer electronic components, high-density designs etc) and are completely unnecessary for keyboard projects.

Therefore, it must always be understood that a single body of uniform tolerances throughout a wide spectrum of projects is unfeasible due to the very nature  and requirements of the different PCB projects. The values suggested here used in these files are only *recommendations* based on experience, having been used in a myriad of projects throughout the Acheron Project and in Gondolindrim's personal and professional projects.

The tolerances used might seem too loose or big, but these are the bare large-scale manufacturable values found through experience using multiple factories.

Adjust the values at your own discretion, but be very mindful of these tolerances as they are imperative for the manufacturing process and feasibility of the final PCB.

The values used divide into two groups: the "factory minimums " and the "recommended minimum" ones.

- "Factory" are the values minimally feasible needed by factories. Different ones will inevitably have different numbers, but through using multiple a common denominator was found. These values should not be used often, but seldomly at the designer's discretion and in ultimate case. Their use will incurr in higher manufacturing fees, larger lead times, more quality check issues (PCBs failing after large production).
- Recommended are the smalles tolerances doable with no fabrication or quality control issues. These can be safely used without incurring in higher costs or major large-scale production issues. There is no particular reason for any of these but experience through usage of many factories. This KiCAD file was set to use the recommended values in its Design Rule Checks and routines; these values were used successfully throughout the Acheron Project and are proven to work.

Keep in mind that these are, after all, minimum values. Always try to stray away from them when there is chance, so as to give you and the factory headroom to work with. The actually used values can very well vary according to your specifications and the capabilities of the factory being used. For the actual values, check references [1-3].

**TLDR** (you should read it all though)
- The values used are based on experience but should be manufacturable at low cost and accepted industry-wide
- Such values *should* be enough for keyboard PCBs
- Manufacturing parameters might change in which case the tolerance values will change accordingly
- You can freely change these parameters around but make sure what those changes entail to, cost and production-wise
- If you are beggining on PCB design or are using a new fab, it might be wise to just use the values here for now

### Notes on copper pours

Many DIY designers will state that the usage of copper pours is perfeccionism; in some cases, designers will argue that the pours are actually detrimental to the design, while I (gondolindrim) disagree with the former I agree with the latter in some respects. Ground pours are an integral part of digital high-speed signal design; since most (if not all) modern keyboards work under USB communication which uses differential pair topology, a ground copper pour is absolutely needed to ensure proper return currents paths, low ground impedance, EMI resistance, efficiency in ESD protection, protection from overheating, and so on. Particularly in keyboard PCBs, however, the copper pours make the PCBs stiffer, reducing what is known as "flex". The way to countermeasure that is by deploying flex cuts (also known as relief cuts) or leaf-spring mounting points. Use copper pours are your discretion but I (Gondolindrim) recommend always using them. My designs make liberal use of such pours even for other signals.

At the bottom-right of the page there is a copper polygon with the settings generally used for the Acheron Project. The values are tuned for general use, but adjustments can be made. Thermal spoke width and relief gap can be adjusted according to the thermal needs -- for THT components or connectors, it is recommended to use higher gaps (0.5mm will do the trick) to avoid bad solder joints.

Avoid using hatched-pattern copper pours. They have their reason to be, but not on keyboard PCBs. They are ugly (yeah, fight me) and don't do what we want them to do here. Always use solid fills. The drawback is that they can make the PCB stiffer, so use flex cuts on the PCB.

### Used tolerances

#### IMPORTANT!
The values and observations here listed consider a two-layer, 1 oz/ft² copper weight PCB setting. A change in these parameters (layers and copper weight) will unequivocably change the minimum values. Values also pertain to the cheapest setting on the referenced manufacturers. Therefore even for a 2-layer 1oz/ft2 PCB tolerances can be different for the "advanced" or "high-difficulty" settings.

DO NOT use these values in other settings; always coordinate properly with the factory for that. All values are given in milimeters. For clarification, read the NOTES ON TOLERANCES.

|	**Tolerance**				|	**Factory minimums**	|	**Recommended minimums**	|
|	:---					|	:---:			|	:---:				|
|	**Track width**				|	0.15			|	0.2				|
| **Copper clearance** (i)			|	0.15			|	0.2				|
| **VIA hole**					|	0.2			|	0.3				|
| **VIA annular width**				|	0.13			|	0.15				|
| **VIA diameter**				|	0.4			|	0.6				|
| **Copper-to-hole clearance** (iii)		|	0.3			|	0.35				|
| **Copper-to-edge clearance**			|	0.2			|	0.5				|
| **Minimum through hole drill**		|	0.2			|	0.3				|
| **Hole-to-hole clearance**			|	0.5			|	0.5				|
| **Silkscreen character height**		|	0.8			|	1.0				|
| **Silkscreen trace width**			|	0.15			|	0.2				|
| **Silkscreen character height-to-trace ratio**|	1:6			|	1:5				|
| **Pad-to-silkscreen clearance**		|	0.15			|	0.2				|
| **Soldermask expansion**			|	0.05			|	0.1				|
| **Minimum soldermask bridge**			|	0.2			|	0.25				|

*All measurements in mm*.

**Observations:**

- [i] Official copper-copper clearances are 0.2mm but not exactly "all copper". Pad-to-pad minimums are 0.5mm in the case of THT pads and 0.2mm for SMD pads.
- [ii] The recommended ratio between silkscreen character height and its trace width so they are clearly legible.
- [iii] The hole-to-copper clearance changes on occasion. For instance, via-to-track and NPTH-to-track clearance is 0.25mm but PTH-to-track is 0.3.
- [iv] The distance of copper-to-edge is a big problem for fabs in designs where traces need to be close to certain slots or the edges, like keyboard PCBs with flex cuts where the PCB traces need to be routed close to the flex cuts for lack of real-estate. This is why this value is much higer than the fabrication ones.
- [v] The 1:6 ratio for silkscreen is OK for large characters but can become unreadable to the naked eye on a 1mm character. A 1:5 ratio is recommended.

### Overall design recommendations

- The first experience with PCB design is always very frustrating and overwhelming. There will be a lot of failed prototypes before you consider yourself a minimally good PCB designer, and there is always something to learn. Since PCB design is a highly technical and skill-oriented field, it can sometimes looks as some sorte of black magic. Do not stray from resilience. The best way to learn PCB design is designing. Some study in the fundamental aspects of electronics and embedded systems is also recommended, but higher education in maths and engineering is absolutely not needed to be a DIY designer.

- Sharing your design and knowledge is one of the most fruitful and effective ways to learn design and gain knowledge. Look at other people's designs and frequently talk to other designers. This ensures a collective knowledge is achieved both in hard and soft skills.

- PCB design involves a lot of creativity and not everyone understands your codes and conventions. Take good care of documenting your PCB. Some tips:
	- (1) Make liberal use of the F.Fab and B.Fab layers. Those should be used for documentation which you want to relay to your fab house. Component pinage and polarity, values, designators, everything goes to make the fab's job easier -- and yours in the process. 
	- (2) Treat the silkscreen layers with very good care. Make sure that the component designators are minimally sized (1mm tall with 0.2mm trace width is advised), visible. Also make sure that the silkscreen indicates the polarity of components either through arrows or symbols. Remember that the physical PCB does not accompany its files; when someone looks at the PCB, component designators and polarities should be very clear and direct.
	- (3) KiCAD allows custom user layers. Use those to indicate screw hole anchors/sizes, plate markings, stabilizer orientation. Document everything because in the eventuality you want to make a revision or correction, all the information is in the files and not somewhere else buried deep down in emails or Discord chats.
	- (4) If you want to share or open-source your design, be kind and make sure all your thought process or ideas are clearly documented and defined. Revising a PCB, much like a computer code or a maths test, requires knowledge that is either a convention or only the author knows.
	- (5) Yes, documenting is boring. Everyone knows that, and yet everyone takes the time to do it because it saves you time and headaches down the line.

- Even though keyboard PCB design is very forgiving in terms of the mistakes and errors you can make, there are some problems that are potentially PCB-breaking. The most common problems for first-timers in keyboard PCB design are (1) USB data traces routing (2) crystal traces routing and (3) bypass capacitors placement and routing. In this order, they are the most problematic parts of designing a keyboard PCB and are the most frequent issues.
	- **(1) USB DATA TRACES:** make sure to route them as short as possible, parallel to one another. Also learn to use the phase skew adjustment tool of KiCAD. Avoid breaking and VIA'ing them as much as possible. If those are needed, do so in a simmetrical manner. If also possible, please read on differential pair  communication (see [4], pages 331-344).
	- **(2) CRYSTAL TRACES:** these are the highest frequency traces on the PCB and should be handled with extreme care. Always place the crystal extremely close to the MCU (in its vicinities) and if possible sharing the same ground (ideally islanded).
	- **(3) BYPASS CAPACITORS:** the precise function and nature of bypass (also called decoupling) capacitors is difficult to explain without higher knowledge in maths and engineering. In layman's terms, bypass capacitors are placed to mitigate noise inherently present in digital circuits due to the natural inductance of traces and the switching characteristic of digital circuitry. Failure to properly place and route these capacitors can lead to inability of the digital circuits to work properly. In the specific case of keyboard PCBs, there are plenty of digital circuits that need decoupling, mainly the microcontroller, RGB underglow LEDs and level shifters. Treat bypass capacitors like you would a crystal: as close to the main device as possible and with short traces. Always look at datasheets, reference manuals and application notes which will generally inform how many capacitors are needed and their respective values and sizes.

## References

- [1] PCBWay manufacturing capabilities. Link: <https://www.pcbway.com/capabilities.html>. Last access august 21, 2021.
- [2] JLCPCB manufacturing capabilities. Link: <https://jlcpcb.com/capabilities/Capabilities>. Last accessed august 21, 2021.
- [3] Elecrow manufacturing capabilities. Link: <https://www.elecrow.com/download/quote/PCB_Specification_FAQ.pdf> . Last access august 21, 2021.
- [4] Sanjeeb Mishra, Neeraj Kumar Singh, Vijayakrishnan Rousseau. System on Chip Interfaces for Low Power Design. Morgan Kaufmann, 2016. ISBN 9780128016305. Accessible via https://www.sciencedirect.com/science/article/pii/B9780128016305000104)
