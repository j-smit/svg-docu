#Scalable Vector Graphics (SVG)

##SVG Canvas
The canvas is the space or area where the SVG content is drawn.  It is rendered on the screen relative to a finite region known as the viewport. Areas of the SVG that lie beyond the boundaries of the viewport are clipped off and not visible.

###Viewport
The viewport is the viewing area where the SVG will be visible.

- You specify the size of the viewport using the width and height attributes on the outermost `<svg>` element. 

		<!-- the viewport will be 800px by 600px -->
		<svg width="800" height="600">
    		<!-- SVG content drawn onto the SVG canvas -->
		</svg>
- In SVG, values can be set with or without a unit identifier. Without (user units) are equivalent to px.
- The supported length unit identifiers in SVG are: `em`, `ex`, `px`, `pt`, `pc`, `cm`, `mm`, `in`, and `percentages`.
- Once the width and height of the outermost SVG element are set, the browser establishes an initial **viewport coordinate system** and an initial **user coordinate system**.

### Initial coordinate system


- **Viewport coordinate system**: The initial viewport coordinate system is a coordinate system established on the viewport.
	- Starts at the top left corner of the viewport at point (0, 0), 
	- the positive x-axis pointing towards the right, 
	- the positive y-axis pointing down, 
	- and one unit in the initial coordinate system equals one "pixel" in the viewport (like CSS box model) 
- **User coordinate system**: The initial user coordinate system is the coordinate system established on the SVG canvas. 
	- This coordinate system is initially identical to the viewport coordinate system—it has its **origin** at the top left corner of the viewport with the positive x-axis pointing towards the right, the positive y-axis pointing down. 
	- Using the `viewBox` attribute, the initial user coordinate system — also known as the **current coordinate system**, or **user space in use ** — can be modified so that it is not identical to the viewport coordinate system anymore.


###Viewbox
The best is to see to `viewBox` as the *real* coordinate system. It is the coordinate system used to draw the SVG graphics onto the canvas

- This coordinate system can be **smaller** or **bigger** than the **viewport**, and it can be **fully** or **partially visible inside the viewport**  too.
- the browser created a default user coordinate system that is identical to it viewport coordinate system.
- Own user coordinate system are created using the `viewBox` attribute.
- If the user coordinate system you choose has the same aspect ratio (ratio of height to width) as the viewport coordinate system, it will stretch to fill the viewport area.
- However, if your user coordinate system does not have the same aspect ratio, you can use the `preserveAspectRatio` attribute to specify whether or not the entire system will be visible inside the viewport or not, and you can also use it to specify how it is positioned inside the viewport.

####Viewbox syntax
The viewBox attribute takes four parameters as a value: 

	viewBox = <min-x> <min-y> <width> <height>

- `<min-x>` & `<min-y>`: values determine the upper left corner of the viewbox
- `width` & `height`: determine the width and height of that viewport. 
- Note here that the width and height of the viewport need not be the same as the width and height set on the parent `<svg>` element.
- A negative value for `<width>` or `<height>` is invalid. 
- A value of zero disables rendering of the element.

*I like to visualize the SVG canvas with a viewBox the same way as Google maps. You can zoom in to a specific region or area in Google maps; that area will be the only area visible, scaled up, inside the viewport of the browser. However, you know that the rest of the map is still there, but it's not visible because it extends beyond the boundaries of the viewport—it's being clipped out.*	
	
#####Viewbox with aspect ratio equal to the viewports aspect ratio

	<svg width="800" height="600" viewbox="0 0 400 300">
    	<!-- SVG content drawn onto the SVG canvas -->
	</svg>

**So, what does `viewbox="0 0 400 300"` exactly do?**

- It specifies a **specific region** of the canvas spanning from a top left point at (0, 0) to a point at (400, 300). 
- The SVG graphic is then **cropped** to that region.
- The region is **scaled up (in a zoom-in-like effect)** to fill the entire viewport. 
- The user coordinate system is mapped to the viewport coordinate system so that—in this case—one user unit is equal to two viewport units (800/400 = 2x & 600/300 = 2x). 
- Anything you draw on th SVG canvas will be drawn relative to the new user coordinate system.

####`preserveAspectRatio` attribute
The `preserveAspectRatio` attribute is used to force a uniform scaling for the purposes of **preserving the aspect ratio of a graphic** and it allows you to specify how to position the viewbox inside the viewport if you don't want it to be centered by default.

#####`preserveAspectRatio` synstax	

	preserveAspectRatio = defer? <align> <meetOrSlice>?

- The `defer` argument is optional, and is used only when you're applying `preserveAspectRatio` to an `<image>`. It is ignored when used on any other element.
- The `align` parameter indicates whether to force uniform scaling and, if so, the alignment method to use in case the aspect ratio of the `viewBox` doesn't match the aspect ratio of the viewport.




##**Grouping** with the `<g>` Element
- `<g>`: stands for group. The group element is used for logically grouping together sets of related graphical elements
- Grouping elements is very useful, not just for organizational and structural purposes. It's particularly useful for when you want to add interactivity or transformations to an SVG graphic that is made up of several items. 
- The <g> element has one more important and great feature: it can have its own `<title>` and `<desc>` tags that help make it more accessible to screen readers, and overall make the code more readable to humans as well
		
		<g id="bird">
    		<title>Bird</title>
    		<desc>An image of a cute little green bird with an orange beak.</desc>
   			 <!-- ... -->
		</g>
			
##**Reusing** elements with the `<use>` element
- The `<use>` element lets you reuse existing elements, giving you a similar functionality to the copy-paste functionality in a graphics editor.
- The `<use>` element takes `x`, `y`, `height`, and `width` attributes, and it references other content using the xlink:href attribute. So if you have defined a group somewhere with a given id, and you want to reuse it somewhere else, you give its URI in an `xlink:href` attribute, and specify the `x` and `y` location where the group's `(0, 0)` point should be moved to.

##Reusing stored elements with the `defs` element
The `<defs>` element is used to define elements that you want to reuse later. Defining elements with `<defs>` is useful for when you want to create sort of a “template” that you want to use multiple times throughout the document. Elements defined inside a `<defs> ` element are not rendered on the canvas except if you “call” them somewhere in the document.

`<defs>` is useful for defining many things, but one of the main use cases is defining patterns like gradients, for example, and then using those gradients as stroke fills on other SVG elements. It can be used to define any elements that you want to render anywhere on the canvas by reference.


##Grouping elements with the `<symbol>` element
The `<symbol>` element combines the benefits of both the <defs> and the <g> elements in that it is used to group together elements that define a template that is going to be referenced elsewhere in the document. Unlike <defs>, <symbol> is not usually used to define patterns, but is mostly used to define symbols such as icons, for example, that are referenced throughout the document.
The <symbol> element has an important advantage over the other two elements: it accepts a viewBox attribute which allows it to scale-to-fit inside any viewport it is rendered in.



## Transform
You can apply transformation to all SVG shapes. You can also apply transformation to the `<g>` element, thus effectively transforming a whole group of elements in one go. It is also possible to transform gradients and fill patterns.