# Julia Workshop

## Prerequisites

### Install Julia

* Download and how to on http://julialang.org
* Install Atom or any other nice editor(e.g. vim, Notepad++)

  ubuntu:
  `sudo snap install atom  --classic`

  install juno

* Download the notebooks into the folder IntroToJulia

  `git clone https://github.com/wibraun/IntroToJulia --branch fhWorkshop`

### Add Julia package for jupyter
```
  import Pkg
  Pkg.add("IJulia")
  # Open up IJulia at this location
  using IJulia
  notebook(detached=true,dir=joinpath(pwd(),"IntroToJulia"))
```

### Test Julia via plot running
```
  import Pkg;
  Pkg.add("Plots")
  # for PyPlots
  #Pkg.add("PyPlot")
  #Pkg.add("PyCall")
  #Pkg.add("LaTeXStrings")
  # for terminal plots
  #Pkg.add("UnicodePlots")
```
### Testing Plots
```
  using Plots
  # example 1
  x = 1:10; y = rand(10);
  plot(x,y)
  # example 2
  x = 1:10; y = rand(10,2);
  plot(x,y)
  # example 3
  x = 1:10; z = rand(10);
  plot!(x,z)
  plot(x,y,title="Daten mal 2",label=["Data 1" "Data 2"],lw=3)
  xlabel!("Random Data") # uses Plots.CURRENT_PLOT -> attribute!(p,value)

  # use different backends
  x = 1:10; y = rand(10,2) # 2 columns means two lines
  plotly() # Set the backend to Plotly
  plot(x,y,title="This is Plotted using Plotly") # This plots into the web browser via Plotly
  gr() # Set the backend to GR
  plot(x,y,title="This is Plotted using GR") # This plots using GR
  unicodeplots() # Set the backend to UnicodePlots
  plot(x,y,title="This is Plotted using UnicodePlots")
```

## Mental Model of Julia

  [link](ucidatascienceinitiative.github.io/IntroToJulia/Html/JuliaMentalModel)

## Julia  Basics

  [link](https://github.com/wibraun/IntroToJulia)

## use OpenModeica from Julia

  [link](https://github.com/OpenModelica/OMJulia.jl)

```
  using OMJulia
  omc = OMJulia.OMCSession()
  omc.sendExpression("loadModel(Modelica)")
  ans = omc.sendExpression("simulate(Modelica.Fluid.Examples.DrumBoiler.DrumBoiler, outputFormat=\"csv\")")
```
### Read Results from OpenModelica

  Package to read MAT doesn't support v4, so we have to use csv
```
  Pkg.add("CSV")
  Pkg.add("DataFrames")
  using CSV
  using DataFrames
  using Plots
  # set plot Backend
  gr() # or plotly()
  res = CSV.File("Modelica.Fluid.Examples.DrumBoiler.DrumBoiler_res.csv")
  df = res |> DataFrame
  plot(df[:time],[df[Symbol("controller.x")],df[Symbol("evaporator.V_l")]])
```
