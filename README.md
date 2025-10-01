//Developed classes allow to perform a 2D ray tracing simulation
//defining reflecting surfaces (2D lines), and absorbing volumes (2D areas)

//First, a reactor must be declared
//Currently, only CircleReactor (a tube) and SegmentsReactor (a convex angles prism) can be declared
//Circle reactors constructor requires a center (Point2D) and a radius (double)
//Segments reactors have an empty constructor, and include several functions to define their geometry:
//    addPoint(point2D)       to define the consecutive points of the prism profile
//    close()                 once all the points are defined, to create the segments and calculate the profile area

//Second, a simulation must be declared
//It is the main class, storing all the geometry and optical properties
//Simulation constructor needs a reactor, an optical density, and the number of rays that will be used
//Once constructed, several functions can be used to complete the geometry definition:
//    addReactor(Reactor *)   It uses a pointer, and not a Reactor instance, to allow polymorphism in reactors functions
//                            The memomry address of a reactor created in the main function can be passed using "&":
//                            CircleReactor r2(point2D(0.0,2.0), 0.7);  case.addReactor(&r2);
//    addCollector(Collector, double) The class collector and its constructors are described below
//                            The second parameter is the reflectance of the collector (0 - 1).
//                            If a given geometry needs different reflectances at different locations,
//                              it needs to be declared as several collectors.

//A collector has an empty constructor, and points will be added until the geometry is fully defined
//    addPoint(point2D)       Works like addPoint in Segments Reactor
//    close()                 Works like close in Segments Reactor (for closed collectors)
//    createSegments()        Equivalent to close, but for opened collectors (CPCs, PTCs...)
//  As most collectors are opened collectors, close() function is called when they are added to the simulation

//Once defined the case geometry, a radiation source must be declared.
// Simulation class offer a simple function to generate parallel beams from the geometric limits of the geometry
//  generateBoxBeams(vector2D) It produces a list of beams (vector<beam>)
// Alternatively, the user can produce a beam or a list of beams, to define other types of sources
// A beam2D constructor needs an origin (point2D), and a direction(vector2D)
// Example code to produce a punctual isotropic source:
//      std::vector<beam2D> punctualSource;
//      for(int i = 0; i < 360; i++)
//        punctualSource.push_back(beam2D(point2D(1.5,2.0), vector2D(cos(M_PI*i/180.0),sin(M_PI*i/180.0))));

//Finally, a list of rays will be declared, using the source (list of beams) and the simulation to construct them.
//The ray constructor, receiving a beam, and a reference to a simulation, traces the ray travel during construction
//Thus, once all the rays are constructed, the ray tracing simulation has finished

//Getting results from the simulation:

//Numerical results: simulation class offers a calculate function, that require the output values to be passed by reference:
//  calculate(int nRays, double& DCF, double& DCF_flux, double& distrib)
//  nRays is the number of rays used to perform the simulation
//  DCF is the Dynamical Concentration Factor, a relation between emitted radiation and available incident radiation
//  DCF_flux is the Dynamical Concentration Factor based on flux, a relation betweeen emitted and absorbed energy
//  distrib is a measure of the homogeneity of incident radiation inside the reactors
//  distrib 0 would mean total homogeneity, with higher values as heterogeneity increases
//  It is designed to be used always comparing designs, with arbitrary units for heterogeneity
//  The class also offers a clear() function to avoid redefining the geometry of the case to perform a new simulation
//  Allowing to quickly modify the source, or the number of rays

//Graphical results: Both the geometry and the rays can produce segments to draw them in any external software
// Using the draw() function, available in collectors, reactors, and rays, the simulation will output to console a list
// of 2D points, with the first column for x coordinates and the second one for y coordinates.

//Some example codes (including all the ones used in the article) are presented in _RayTracing_Examples.cpp, intended to be used as a base.

