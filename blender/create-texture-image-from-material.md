# Generate (Bake) texture image from Material

Sometimes we need to create materials and export them to external programs that not recognizes the blender material format.

One possible solution is to generate a texture from material composition.

To do so we need to follow some steps:

1) generate a Smart UV Project
  ```
    Select the desired object by left clicking on it
    Enter in edit mode (hotkey - tab)
    Select all vertices (hotkey - a)
    Generate Smart UV Project (hotkeys - u > s) or by clicking on UV menu > Smart UV Project
    Click on OK to accept defaults
  ```

2) Bake the texture
   ```
      On the right panel select Render Properties tab
      Change Render Engine to Cycles
      Expand Bake Section
      Change Bake Type to Combined
      Click Bake
   ```

3) Save Texture image
   ```
      On UV editor tab click image > save as and select the desired location
   ```

## Conclusion
After that you will have a generated texture from the material that was applied to the object.
This texture can be used as image for any other material inside blender and exported to other formats