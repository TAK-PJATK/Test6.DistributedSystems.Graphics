# Test 6. Distributed Systems. Graphics

## Distributed Systems

In this lecture, we will discuss an important area of technology that has not yet been covered in this course: networking and **distributed systems**. These technologies allow for the delegation of computing tasks or data storage to external machines.

Networking technology enables the combination of multiple computers into a single **distributed system**, so that all the hardware resources can be utilized for more complex tasks. The term _distributed_ is used here to differentiate this approach from _centralized_ systems, where all information exchange occurs through a single central computer. Another alternative to the distributed model is a single _supercomputer_, which is equipped with extremely powerful components. These approaches are illustrated in the following diagrams:



Figure 1. Types of large-scale computer systems.

### Pros and cons

The advantage of distributed systems over centralized ones is typically better resilience to the failure of a single node or network overload. Compared to supercomputers, the advantages are:

- Increasing computing power at relatively lower costs;
- Better resilience to hardware failures;
- Quite easy scalability of the system (when more computing power is needed, additional nodes can be added).

However, a weakness of distributed systems compared to supercomputers is the limited computing power of a single node. This limitation is significant in cases where complex tasks require parallelism, which can be difficult to achieve, and where longer latencies in transmitting data between threads are not acceptable.

For these reasons, supercomputers are typically used for specific, highly demanding tasks (e.g., complex meteorological models, intensive training of artificial intelligence), while distributed systems are more often utilized for parallel execution of multiple, relatively simpler, and often mutually independent tasks.

Another disadvantage of distributed systems is the necessity of managing their complexity, often in a decentralized manner. This brings a whole range of challenges, including:

- Handling **asynchronous communication**, where message delivery can take a significant and unpredictable amount of time or may fail altogether;
- **Replicating** information across different nodes to ensure availability even in the event of local hardware failures, while also ensuring the **consistency** of this information across different replicas;
- Administering the system on the software level (e.g., ensuring compatibility between various operating systems or different versions of the same program, and/or implementing an appropriate update strategy).

These are all very interesting topics, though unfortunately they are outside the scope of this course.

Finally, let us note that many networks used in practice have an "intermediate" structure; that is, they are distributed systems built from _powerful_ node machines that allow efficient execution of more demanding tasks, though they still fall far behind the top supercomputers. In such cases, when we want to expand such a system, we can choose between two strategies: **horizontal scaling** (adding new nodes, making the system "more distributed") or **vertical scaling** (improving the efficiency of individual nodes, which moves more toward centralization).

### Organization at Software Level

We will now discuss a few main types of logical architecture of computer systems. Depending on the ownership landscape of nodes and their roles, these are:

- **Peer-to-peer architecture (P2P):** Network nodes have diverse owners (e.g., often they will be personal computers of unrelated people) and all have the same role. Each node may begin to be served by another node upon establishing an appropriate bilateral connection between them. This model is used by popular torrent systems: each node in the network can send a request and start downloading data from any other node; similarly, each node can be sending files. Another example is instant messengers like Skype.

- **Client-server architecture:** This architecture distinguishes a central server (which can be either a single computer or, increasingly often, a whole computer system) that exposes specific functionality to other (typically numerous) nodes, called clients. The server is responsible for (almost) the entire logic related to its functionality; in practice, the client side may contain an additional user interface. For example, although the essential operation while browsing the internet is sending requests and receiving responses from HTTP servers, displaying the received web page legibly is also important. This task is handled by a web browser (the user interface) installed on the user's computer (the client side).

When the server itself is a whole computer system, there are more approaches to choose from:

- **Cluster architecture:** The server consists of many machines, each of which takes a share of tasks similar to each other. In this model, the server itself becomes a computer network, though when speaking of a cluster, we typically mean a network of machines located close to each other. (In the case of servers distributed over large distances, e.g., diverse continents, we would typically describe them as a network of local clusters). In all cases, it becomes important to effectively (usually meaning evenly) spread the work among the nodes, which is the responsibility of a load balancer.

- **Multi-layer architecture:** Here, the functionality of the server is split into separate modules (or layers) assigned to different machines. In the simplest case, the server is split into a database and a web server communicating with the client. However, one can also distinguish a larger number of specialized layers (for example: authorization layer, cache, task scheduler).

In practice, naturally, we can encounter mixed architectures, e.g., a multi-layer server in which the layer dealing with the main computations (and sometimes other layers) is a distributed system of a cluster architecture.

Let us also mention the **grid architecture**, in which a distributed system is built of diverse machines, often belonging to various owners, who agree to dedicate some computing resources to a shared goal. Modern examples include volunteer projects for specific intensive computations (e.g., Folding@home, a network performing bioinformatics simulations for the purpose of fighting certain classes of diseases). Another example is blockchain networks for verifying cryptocurrencies (e.g., Bitcoin).

In the context of our classifications above, grid architectures do not allow for simple labeling as a whole class; their classification depends on the specific case. Verifying cryptocurrencies typically occurs in a decentralized peer-to-peer network. In contrast, a typical citizen science project may be regarded, on one hand, as a single distributed server whose service (computation) is used by scientists at the root of the project. On the other hand, the individual machines involved in those computations are commonly described as clients that connect to a server (or servers) to receive subsequent shares from the central pool of tasks. (In this way – quite unusually – it is the server that requests work from the client).

### Services in the Cloud

In modern distributed systems, special focus is usually placed on:

- A specific service delivered by the system (for example: storing users' files, providing videos on demand, providing an environment for the customer's web app operation);
- Ease of use, understood as hiding the entire technical layer from the customers and leaving them with just a well-defined user interface through which the service is accessed;
- High availability, understood as minimizing the service downtime (due to any failures) and minimizing the waiting time for connecting to the server and completing tasks (typically achieved by distributing servers across various world regions).

This philosophy of building computer systems is commonly known as service-oriented architecture. In the context of distributed servers of a global scope (i.e., possibly close to all users) and an implementation hidden behind an abstract interface, this technology is commonly referred to as "the cloud."

Below, you can see a graph illustrating the main providers of cloud solutions. As it shows, in the case of cloud computing, we typically observe a single owner controlling an entire ecosystem of services and managing the entire system in a uniform way. The service delivered by the cloud provider is usually much more extensive than in the case of grid systems.

![Figure 2. The leading providers of cloud services.](#)

Figure 2. The leading providers of cloud services.

On the level of the technical network structure, as a cloud system typically involves many users with independent computing tasks, cloud servers are almost always distributed systems; more specifically, they are clusters or distributed networks of clusters.

### Levels of Service

Considering the type of services delivered, we can distinguish three main models of cloud systems:

- **IaaS (Infrastructure as a Service):** Here, the provider offers infrastructure, meaning access to servers and maintaining them in an operational state, but no accompanying software. Physically, the computers could be located at the customer's headquarters, or even be the customer's property. Typically, however, the customer remotely uses hardware belonging to the provider. In this latter case, it's common practice to place customers' processes on virtual machines (special programs emulating a given computer model and an operating system), which allows, for example, enforcing isolation between processes from different customers running on a single physical machine by running those processes on separate virtual machines.

- **PaaS (Platform as a Service):** In addition to the infrastructure, the provider offers an environment, consisting mostly of software related to the servers, network, and databases. This is typically used by developers for building web applications, abstracting away from the technical details of the underlying hardware. This layer also traditionally contains utilities oriented towards software engineering, such as frameworks for automated testing, deployment, and monitoring the quality of service as perceived by the end user.

- **SaaS (Software as a Service):** Here, the provider offers specific applications that users utilize for particular purposes. An example of this is Dropbox, which allows for uploading, viewing, and downloading files via a web browser (thus creating disk space "in the cloud").

As individual private users, we often use cloud solutions without even realizing it. This happens whenever we send an email, listen to music from the internet, play online games, or watch movies. The picture becomes somewhat different when a cloud system is used by a company. Here, delegating some tasks to the cloud provider can help in areas such as:

- Data analysis (regarding customer choices, application performance, etc.);
- Delivering the product to end users;
- Ensuring scalability of hardware (both upwards and downwards to avoid overcharging for unused hardware resources);
- Creating and maintaining data backups;
- Networking security issues;
- Testing the product and its new versions;
- Applying the DevOps culture, which emphasizes cooperation between development teams and the teams responsible for product health and maintenance;
- Applying the agile programming concept, which assumes frequently confronting the product with actual users' needs and frequently adjusting the project.

The concept of the cloud is also related to the notion of virtualization, though we are now considering this word in a much broader sense than just RAM virtualization, which we discussed in an earlier lecture. Generally, virtualizing means emulating some resources by using other ones, often through appropriate software. In the case of cloud solutions, this is typically implemented by the aforementioned virtual operating systems, which ensure the isolation of a single customer's processes and may also provide a remote desktop that lets the user connect to the remote server ("in the cloud") and use the emulated desktop just as if it were part of a standard operating system. Network virtualization may also take place, splitting the capacity of physically existing network connections into independent virtual channels.

## Graphics

In this lecture, we will look at the most important input/output devices which have not yet been discussed: graphics cards, screens, and printers. (Of course, there are many other kinds of input/output devices that we frequently use, but we will restrict our discussion to these types).

### Graphics Cards

A graphics card, as the name suggests, is responsible for processing the images displayed on the screen. This is not an easy task. In the lecture "Encoding Data," we described how a computer stores and operates on integers, floats, and texts. Working with images also requires encoding them in binary form. The easiest way would be to encode the color of each pixel separately (as is done in BMP files); however, this method is very computationally inefficient. (The importance of efficiency is well known by every computer gamer—it is the graphics card's computing power that often decides if the game will operate smoothly or lag). Therefore, the standard data representations used in graphics cards are more sophisticated.

#### Types of Graphics Cards

While a graphics card can be found in almost every computer, it can take one of the following two forms:

- **Integrated graphics card:** The word "integrated" represents the fact that it is part of the processor.
- **Dedicated graphics card:** A separate module, usually exchanging data via a PCI Express bus (discussed in previous lectures).

##### Advantages of an Integrated Card

- Lower price.
- Lower power consumption, leading to:
  - Smaller weight.
  - Longer battery running time in laptops.

##### Advantages of a Dedicated Graphics Card

- Greater computing power.
- Option to be replaced by a better model later.

#### Hardware Properties

The most important component of a graphics card is the graphics processing unit (GPU). In colloquial speech, the phrases GPU and graphics card are often used interchangeably. A GPU shows many resemblances to the computer CPU, as both units play a similar role: performing computations. However, the CPU is a general-purpose unit, fit for a variety of tasks, including serial tasks. Every single core of a CPU can execute tasks on its own, serving as a stand-alone processing unit. On the other hand, the GPU architecture is optimized for highly parallel and vector computations, performed simultaneously on many cores. As a result, a GPU will typically contain many more cores than a CPU, often going into hundreds or even thousands.

Just like the CPU, a graphics card needs to use memory. Dedicated cards are equipped with their own (separate) hardware, whose size may currently reach even 24 GB, though typically is below 10 GB. For moderate applications, 2-4 GB should suffice. Graphics cards integrated with the CPU do not have such dedicated memory chips; instead, they are given some area of the main RAM memory by the processor, of size depending on hardware settings, and sometimes also on user preferences. For some cards, an option to control their memory size will be available through their drivers; this parameter can also be managed in the UEFI/BIOS settings.

#### Applications

Although the main task of graphics cards is graphics imaging, they have also found applications in other areas requiring heavy computations which allow for intensive parallelization. These include implementations of so-called deep learning or artificial intelligence, especially when the data being handled are very large.

In recent years, graphics cards have found a new application in cryptocurrency mining. Sadly, this has led to a substantial increase in graphics cards prices and has played a role in increasing global electricity consumption. Additionally, we observe an increased number of security attacks on computers, where hackers use the overtaken machines as cryptocurrency mines.

Dedicated graphics cards also allow working with more connected monitors than integrated cards, which often have a limit of two.

### Screens

We will now consider the devices for displaying images, which may be called screens, displays, or monitors. (All these terms largely overlap, though a monitor sounds more like a stand-alone device, not including, for example, the display/screen of a smartphone). Before describing the main technologies of manufacturing them, let us discuss some aspects common to all of them.

#### Pixels and the RGB Space

In nearly all modern displays, the image is built from pixels—tiny dots of color—typically laid out in a rectangular array. 

A single pixel is usually capable of displaying any color in the device-supported color space, which is enabled by building it from three sub-pixels: a red, green, and blue one. These sub-pixels are monochromatic (i.e., they shine with varying intensity but in a fixed color).

The choice of sub-pixel colors results from the laws of optics regarding mixing colors of light, which are based on the anatomy of the human eye that employs a color recognition system based on three types of cone cells, as shown in the following diagram:

![Figure 1. The diagram of mixing colors of light.](http://en.wikipedia.org)

The above image can be obtained by placing a white screen in a fully dark room and then using three colored torches (red, green, and blue) to illuminate three partially overlapping areas. As we see, all three primary colors combined together give white light; combining pairs produces other colors.

Generally, almost every color recognizable by the human eye can be obtained as a combination of lights in the three primary colors, just by appropriate adjustment of their intensities. This fact is the basis of the RGB (Red-Green-Blue) color system, used in screens to display various colors, and more generally, in computers to describe colors. In the most popular version, the intensity of each of the primary lights is described by an integer between 0 (none) and 255 (maximum intensity), producing a space of $$256^3 \approx 16.7$$ million possible colors. For example, the triple (150, 30, 50) will denote the color [example color]. (Hexadecimal notation is also commonly used here: for our example, that would be #961E32).

One of the main properties of a screen from the user's viewpoint is its resolution, which means the number of pixels displayed in the current operating mode. For example, Full HD resolution means a rectangular array of 1920 × 1080 pixels. Modern computer screens can typically operate either in their native resolution (the one they have been designed for) or in a lower one (i.e., using fewer pixels), which requires scaling the image and may negatively affect its quality.

#### Manufacturing Technologies

##### CRT

The generation of CRT screens has essentially gone out of fashion. In these models, the key role is played by three electron guns emitting beams of electrons which hit a layer of phosphor (a material that emits light when activated by another kind of energy, not to be confused with the chemical element phosphorus). The exact place where the electron beam will hit the screen is controlled by a deflecting element (using a magnetic field). Each of the electron beams periodically "runs through" all the pixels on the screen; the displayed image is stable because the phosphor continues to emit light for a short time after being activated.

Using a shadow mask—a metal barrier with a grid of round holes (corresponding to pixels), placed at some distance from the screen—allows for better control of the target of the electron beams. This is even more important given that different points of the screen are covered with phosphor shining in different RGB colors; the placement of electron guns and shadow mask holes is chosen so that, for example, the "red-light" gun can hit only those points of the screen covered with the red-glowing phosphor.

##### Disadvantages of CRT Screens

- High power consumption.
- Large size.
- Eye fatigue (due to radiation).
- Poor brightness and contrast.
- Issues with image geometry.

##### LCD

Liquid-crystal displays (LCD) have no electron gun, enabling a substantial reduction in their mass and shape to nearly flat. Light emitted by a flat source (e.g., a system of gas-discharge CCFL lamps) falls on a polarizing filter, which lets through only light waves of a specific orientation. Then, there comes a layer of liquid-crystal molecules, in which transistors turn individual pixels on or off. By applying different voltage levels, we manipulate the pattern formed by these molecules, which affects the polarization of light after traversing the liquid-crystal layer; that in turn determines whether this light will be blocked by another polarizing layer. Finally, single sub-pixels contain color filters that give the light its desired color (red, green, or blue).

There are a few technologies for managing the patterns of crystals, affecting suitable viewing angles for the screens.

A newer generation of liquid-crystal displays has replaced the illuminating CCFL lamps with light-emitting diodes (LED). Such devices are often called LED displays. The advantages of LED backlighting, compared to CCFL, are:

- Better contrast (enabled by smaller size of diodes, allowing selective dimming in darker areas of the image).
- Better viewing angles.

##### Disadvantages of LED Backlighting

- Higher price compared to traditional LCD displays.

##### Advantages of LCD/LED Displays over CRT

- Lower power consumption.
- Reduced size and weight.
- Less eye fatigue.
- Improved brightness and contrast.
- Better image geometry.

##### Disadvantages of LCD/LED Displays

- Worse representation of colors.
- Worse image quality when viewed from sharp angles.
- Longer response time (except for the best LED models).
- Worse quality for non-native resolutions.

#### Other Technologies

##### Plasma

Light in plasma screens is generated by ionizing gas, which, when brought to a plasma state, emits ultraviolet light. Every subpixel is a chamber for such gas, filled with an appropriate phosphor, which upon activation by UV light begins to shine in the desired primary color.

##### OLED

OLED screens and their improved versions (AMOLED, SUPER AMOLED) are examples of "true LED displays," where the role of subpixels is played directly by diodes of various colors. This means there is no need for additional backlight polarization. These technologies are mainly used in small mobile devices due to their higher price.

##### Comparison of Plasma and OLED Technologies with LCD Screens

| Technology | Advantages                           | Disadvantages                                           |
|------------|---------------------------------------|---------------------------------------------------------|
| Plasma     | Thinner, better viewing angles, better contrast, longer lifetime, better mechanical resistance | Heavier, higher power consumption, uneven usage of phosphor, image blinking, noise (noticeable at higher altitudes) |
| OLED       | Better viewing angles, better contrast (and color space), shorter refresh time, no mercury used | Higher power consumption, more fragile, shorter lifetime, development restricted by patents |

#### Ports

A screen is typically connected to a modern computer by one of the following ports:

- **USB:** Discussed in the lecture "Non-volatile storage."
- **HDMI:** A standard for transmitting audio and video signals (also in high quality, without compression) with a throughput of up to 48 Gb/s. It allows using longer cables, though this increases the chance of data corruption.
- **DisplayPort:** Mainly for connecting the computer to a screen or a home theater system, allowing bidirectional transfer up to 80 Gb/s. The transmitted signal may be protected from copying (with DPCP technology) or unauthorized use (with DRM technology).

### Printers

#### Printer Colors: The CMYK Space

Similar to screens, printing technology is based on colored pixels, whose color is obtained by combining various intensities of selected primary colors. The difference in primary colors used lies in the nature of printing inks, which behave more like filters than light sources, leading to darker results. Thus, the standard in modern printing is the CMY (Cyan-Magenta-Yellow) palette, which differs from the RGB set by swapping the roles of primary and secondary colors.

##### Mixing Colors of Printing Ink

![Figure 2. The diagram of mixing colors of printing ink.](http://en.wikipedia.org)

The above image can be obtained by simultaneously placing three circles of printing ink (cyan, magenta, and yellow) on a white sheet of paper. The primary CMY colors coincide with the secondary light colors in the RGB model, and combining pairs of CMY inks leads to RGB primary colors. In practice, this correspondence may not be perfect due to technological differences between ink and diode producers.

##### The CMYK Model

In a perfect world, a combination of cyan, magenta, and yellow would yield black; however, such "blackness" often turns into a dirty brown. Moreover, pure black ink can be produced much cheaper than by mixing three colored ones. Therefore, an additional black ink component, denoted by K (key), is introduced, forming the CMYK model. Each color is described with four numbers, specifying the intensity of the component colors in percent (from 0 to 100). Printing typically occurs in the following sequence: black, magenta, cyan, and finally yellow. An important aspect here is the high transparency of each ink layer.

#### Printer Types

We distinguish three main types of printers:

- **Dot matrix printers:** These operate by moving paper under an ink ribbon which is pushed down by a needle to contact the paper at specific points. This mode of operation resembles that of traditional printing machines. While less commonly used today, dot matrix printers still have niches, such as printing multiple copies at once (using carbon paper), making them a cost-effective choice for printing invoices.
- **Ink printers:** These place drops of CMYK ink on the paper. While the devices themselves are relatively cheap, their maintenance costs (i.e., the cost of ink) are high.
- **Laser printers:** These print by transferring a colorizing powder (toner) from an electrified roller to the paper. A laser beam is used to discharge specific locations of the roller, making them no longer attract the toner. These devices are more expensive than ink printers but have lower running costs.

#### Connectivity

Printers may communicate with the computer via a USB cable, as discussed in the lecture "Non-volatile storage." Increasingly more models also support wireless communication through protocols such as:

- **Bluetooth:** Uses radio waves for data transmission between paired devices, requiring little electric power but having a relatively short communication range (up to 10 meters for the most common class of devices). The newest Bluetooth standards achieve transmission rates up to 50 Mb/s.
- **WiFi:** Also uses radio waves but has a larger range (up to about 90 meters), higher power consumption, faster transmission, and no concept of device pairing. Once a device is connected to a local network, it can essentially communicate with any other device in that network, requiring an intermediate device called a router.

### Computers and Electric Power

To close the lecture, we will mention two issues related to using electric power in computers.

#### Failover Supply

The first issue is the threat of a power outage. Interrupting operation is unacceptable in critical infrastructure (e.g., hospitals) and even in home conditions, as it may pose a risk of damaging the hardware (e.g., a sudden power outage during SSD operation). The solution is to use a failover (or uninterruptible) power supply, known as UPS.

Besides keeping electricity on during an external power outage, UPS devices protect connected hardware from disruptions like sudden changes in network voltage. There are three main kinds of UPS devices:

- **Online:** Completely separates the protected device from the external network, shielding it from outages and voltage disruptions. This variant involves greater energy losses, more noise, a shorter lifetime, and a higher price.
- **Offline:** The device consumes electricity directly from the external network, and UPS batteries are only charged when necessary. This means minimal energy losses and lack of noise under normal conditions. A switch to failover electricity occurs during an external power outage, with a transition duration of about 10 milliseconds. This type does not protect against unexpected voltage changes.
- **Line-interactive and Line-interactive AVR:** Improved variants of the offline solution, offering some voltage stabilization. The reaction time in case of outage is shorter than in offline UPS, at about 5 milliseconds.

#### Cooling

Another issue related to electricity is the heat emitted during computer operation, which can overheat and damage components. Stable operation requires an efficient cooling mechanism.

The components with the largest power consumption are processors and graphics cards, which are equipped with cooling systems typically containing:

- **Radiator:** A system of metal plates transferring heat from the processor or graphics card to the surrounding air.
- **Fan:** Pushing the air into circulation facilitates heat transmission further away. The fan's rotation speed depends on the current temperature. If the fan is already at full speed and the temperature continues to rise, the computer will automatically reduce its efficiency or switch to suspend mode or shutdown. The exact temperature thresholds can be adjusted in UEFI/BIOS settings, but raising them too high risks hardware damage. This is called active cooling, opposed to passive cooling, which involves just a radiator (without a fan). Passive cooling is rarely used in modern computers.

Another solution is liquid cooling, which involves setting up a circulation of some liquid (e.g., purified water) in a closed loop, alternatingly passing near the hottest computer components and much cooler air. In this scheme, water absorbs the heat from the components and releases it into the air. Advantages of liquid cooling include noise reduction and better cooling efficiency, but it also involves increased complexity and volume of the cooling system.

