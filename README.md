# Steck LÜCK spare brick
> Spare part 3D model for a German kids board game [Steck LÜCK](https://www.westermann.de/produktfamilie/STECKLUEK/)

![preview](/image/lueck-spare-part-preview.png)

The 3D model was made with [Cad-Query](https://cadquery.readthedocs.io/) 
a Python library for building parametric 3D CAD models.  
To edit the file you can use the [CQ-editor](https://github.com/CadQuery/CQ-editor).

> 🔎 [3D Preview](https://tobsef.github.io/lueck-spare-part-3d-model/)

## 💾 Downloads
### [lueck-spare-part.stl](lueck-spare-part.stl)
### [lueck-spare-part.py](lueck-spare-part.py)

## ✏ Code
> 💡 Unit of all dimensions is millimeter [mm]
### [`lueck-spare-part.py`](lueck-spare-part.py)
```python
import cadquery as cq

outerSide = 39 # Outer side length
innerSide = 26
wall = 2
height = 6.38
heightUnderShell = height - wall

# Small bulges for a better lock
nobbleRadius = 0.8
nobblePos = 4.8 # Distance from corner
nobbleCorrection = 0.15 # Higher values means smaller nobble height
sideNobbelA = (innerSide + 2 * wall) - nobbleCorrection
sideNobbelB = sideNobbelA - 2 * nobblePos
nobbeRadius = 0.4

sideShell = outerSide - 2 * wall
onPlate = (True, True, False)

nobbels = (cq.Workplane().rect(sideNobbelA, sideNobbelB, forConstruction=True)
           .vertices().circle(nobbeRadius).clean()
           .extrude(heightUnderShell))
nobbels = nobbels.union(nobbels.rotateAboutCenter(axisEndPoint=(0, 0, 1), angleDegrees = 90))

brick = (cq.Workplane()
         .box(sideShell, sideShell, heightUnderShell, centered=onPlate)
         .faces("+Z").shell(wall)
         .union(
    cq.Workplane()
    .box(innerSide, innerSide, heightUnderShell, centered=onPlate)
    .faces("+Z").shell(wall)
).union(nobbels)
         .cut(
    cq.Workplane()
    .box(innerSide, innerSide, heightUnderShell))
         .edges("<Z").fillet(0.8))

show_object(brick, options={"color": (64, 164, 223)}, name='luek game brick')
```