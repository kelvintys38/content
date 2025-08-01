---
title: Paths
slug: Web/SVG/Tutorials/SVG_from_scratch/Paths
page-type: tutorial-chapter
sidebar: svgref
---

{{ PreviousNext("Web/SVG/Tutorials/SVG_from_scratch/Basic_shapes", "Web/SVG/Tutorials/SVG_from_scratch/Fills_and_strokes") }}

The {{SVGElement('path')}} element is the most powerful element in the SVG library of [basic shapes](/en-US/docs/Web/SVG/Tutorials/SVG_from_scratch/Basic_shapes). It can be used to create lines, curves, arcs, and more.

Paths create complex shapes by combining multiple straight lines or curved lines. Complex shapes composed only of straight lines can be created as [`<polyline>`](/en-US/docs/Web/SVG/Tutorials/SVG_from_scratch/Basic_shapes#polyline) elements. While `<polyline>` and `<path>` elements can create similar-looking shapes, `<polyline>` elements require a lot of small straight lines to simulate curves and don't scale well to larger sizes.

A good understanding of paths is important when drawing SVGs. While creating complex paths using an XML editor or text editor is not recommended, understanding how they work will allow to identify and repair display issues in SVGs.

The shape of a `<path>` element is defined by one parameter: {{ SVGAttr("d") }}. (See more in [basic shapes](/en-US/docs/Web/SVG/Tutorials/SVG_from_scratch/Basic_shapes).) The `d` attribute contains a series of commands and parameters used by those commands.

Each of the commands is instantiated (for example, creating a class, naming and locating it) by a specific letter. For instance, let's move to the x and y coordinates (`10`, `10`). The "Move to" command is called with the letter `M`. When the parser runs into this letter, it knows it needs to move to a point. So, to move to (`10`, `10`) the command to use would be `M 10 10`. After that, the parser begins reading for the next command.

All of the commands also come in two variants. An **uppercase letter** specifies absolute coordinates on the page, and a **lowercase letter** specifies relative coordinates (e.g., _move 10px up and 7px to the left from the last point_).

Coordinates in the `d` parameter are **always unitless** and hence in the user coordinate system. Later, we will learn how paths can be transformed to suit other needs.

## Line commands

There are five line commands for {{SVGElement("path")}} nodes. The first command is the "Move To" or `M`, which was described above. It takes two parameters, a coordinate (`x`) and coordinate (`y`) to move to. If the cursor was already somewhere on the page, no line is drawn to connect the two positions. The "Move To" command appears at the beginning of paths to specify where the drawing should start. For example:

```plain
M x y
(or)
m dx dy
```

In the following example there's only a point at (`10`, `10`). Note, though, that it wouldn't show up if a path was just drawn normally. For example:

```html live-sample___move-to
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
  <path d="M10 10" />
</svg>
```

```html hidden live-sample___move-to
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <circle cx="10" cy="10" r="3" fill="red" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___move-to
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('move-to', 100, 130) }}

There are three commands that draw lines. The most generic is the "Line To" command, called with `L`. `L` takes two parameters—x and y coordinates—and draws a line from the current position to a new position.

```plain
L x y
(or)
l dx dy
```

There are two abbreviated forms for drawing horizontal and vertical lines. `H` draws a horizontal line, and `V` draws a vertical line. Both commands only take one parameter since they only move in one direction.

```plain
H x
(or)
h dx

V y
(or)
v dy
```

An easy place to start is by drawing a shape. We will start with a rectangle (the same type that could be more easily made with a {{SVGElement("rect")}} element). It's composed of horizontal and vertical lines only.

```html live-sample___rectangle-lines
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 10 H 90 V 90 H 10 L 10 10" />
</svg>
```

```html hidden live-sample___rectangle-lines
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <circle cx="10" cy="10" r="3" fill="red" />
    <circle cx="90" cy="90" r="3" fill="red" />
    <circle cx="90" cy="10" r="3" fill="red" />
    <circle cx="10" cy="90" r="3" fill="red" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___rectangle-lines
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('rectangle-lines', 100, 130) }}

We can shorten the above path declaration a little bit by using the "Close Path" command, called with `Z`. This command draws a straight line from the current position back to the first unclosed point (first point after the last `Z` command if there is one, or the first point in the path otherwise), and closes off the path with a line joint. It is often placed at the end of a path node, although not always. There is no difference between the uppercase and lowercase command.

```plain
Z
(or)
z
```

So our path above could be shortened to:

```xml
<path d="M 10 10 H 90 V 90 H 10 Z" fill="transparent" stroke="black" />
```

The relative forms of these commands can also be used to draw the same picture. Relative commands are called by using lowercase letters, and rather than moving the cursor to an exact coordinate, they move it relative to its last position. For instance, since our rectangle is 80×80, the `<path>` element could have been written as:

```xml
<path d="M 10 10 h 80 v 80 h -80 Z" fill="transparent" stroke="black" />
```

The path will move to point (`10`, `10`) and then move horizontally 80 points to the right, then 80 points down, then 80 points to the left, and then back to the start.

In these examples, it would probably be more intuitive to use the {{SVGElement("polygon")}} or {{SVGElement("polyline")}} elements. However, paths are used so often in drawing SVG that developers may be more comfortable using them instead. There is no real performance penalty or bonus for using one or the other.

## Curve commands

There are three different commands that can be used to create smooth curves. Two of those curves are [Bézier curves](/en-US/docs/Glossary/Bezier_curve), and the third is an "arc" or part of a circle. You might have already gained practical experience with Bézier curves using path tools in Inkscape, Illustrator or Photoshop. There are an infinite number of Bézier curves, but only two are available in `<path>` elements: a cubic one, called with `C`, and a quadratic one, called with `Q`.

### Bézier Curves

The cubic curve, `C`, is the slightly more complex curve. Cubic Béziers take in two control points for each point. Therefore, to create a cubic Bézier, three sets of coordinates need to be specified.

```plain
C x1 y1, x2 y2, x y
(or)
c dx1 dy1, dx2 dy2, dx dy
```

The last set of coordinates here (`x`, `y`) specify where the line should end. The other two are control points. (`x1`, `y1`) is the control point for the start of the curve, and (`x2`, `y2`) is the control point for the end. The control points essentially describe the slope of the line starting at each point. The Bézier function then creates a smooth curve that transfers from the slope established at the beginning of the line, to the slope at the other end.

```html live-sample___cubic_bezier_curves
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 10 C 20 20, 40 20, 50 10" stroke="black" fill="transparent" />
  <path d="M 70 10 C 70 20, 110 20, 110 10" stroke="black" fill="transparent" />
  <path
    d="M 130 10 C 120 20, 180 20, 170 10"
    stroke="black"
    fill="transparent" />
  <path d="M 10 60 C 20 80, 40 80, 50 60" stroke="black" fill="transparent" />
  <path d="M 70 60 C 70 80, 110 80, 110 60" stroke="black" fill="transparent" />
  <path
    d="M 130 60 C 120 80, 180 80, 170 60"
    stroke="black"
    fill="transparent" />
  <path
    d="M 10 110 C 20 140, 40 140, 50 110"
    stroke="black"
    fill="transparent" />
  <path
    d="M 70 110 C 70 140, 110 140, 110 110"
    stroke="black"
    fill="transparent" />
  <path
    d="M 130 110 C 120 140, 180 140, 170 110"
    stroke="black"
    fill="transparent" />
</svg>
```

```html hidden live-sample___cubic_bezier_curves
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference"></g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___cubic_bezier_curves
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");

// prettier-ignore
const points = [
  [[10, 10], [20, 20], [40, 20], [50, 10]],
  [[70, 10], [70, 20], [110, 20], [110, 10]],
  [[130, 10], [120, 20], [180, 20], [170, 10]],
  [[10, 60], [20, 80], [40, 80], [50, 60]],
  [[70, 60], [70, 80], [110, 80], [110, 60]],
  [[130, 60], [120, 80], [180, 80], [170, 60]],
  [[10, 110], [20, 140], [40, 140], [50, 110]],
  [[70, 110], [70, 140], [110, 140], [110, 110]],
  [[130, 110], [120, 140], [180, 140], [170, 110]],
];

for (const curvePoints of points) {
  for (const p of curvePoints) {
    const circle = document.createElementNS(
      "http://www.w3.org/2000/svg",
      "circle",
    );
    circle.setAttribute("cx", p[0]);
    circle.setAttribute("cy", p[1]);
    circle.setAttribute("r", 1.5);
    circle.setAttribute("fill", "red");
    g.appendChild(circle);
  }
  const line1 = document.createElementNS("http://www.w3.org/2000/svg", "line");
  line1.setAttribute("x1", curvePoints[0][0]);
  line1.setAttribute("y1", curvePoints[0][1]);
  line1.setAttribute("x2", curvePoints[1][0]);
  line1.setAttribute("y2", curvePoints[1][1]);
  line1.setAttribute("stroke", "red");
  g.appendChild(line1);
  const line2 = document.createElementNS("http://www.w3.org/2000/svg", "line");
  line2.setAttribute("x1", curvePoints[2][0]);
  line2.setAttribute("y1", curvePoints[2][1]);
  line2.setAttribute("x2", curvePoints[3][0]);
  line2.setAttribute("y2", curvePoints[3][1]);
  line2.setAttribute("stroke", "red");
  g.appendChild(line2);
}

let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('cubic_bezier_curves', 190, 190) }}

The example above creates nine cubic Bézier curves. As the curves move toward the right, the control points become spread out horizontally. As the curves move downward, they become further separated from the end points. The thing to note here is that the curve starts in the direction of the first control point, and then bends so that it arrives along the direction of the second control point.

Several Bézier curves can be strung together to create extended, smooth shapes. Often, the control point on one side of a point will be a reflection of the control point used on the other side to keep the slope constant. In this case, a shortcut version of the cubic Bézier can be used, designated by the command `S` (or `s`).

```plain
S x2 y2, x y
(or)
s dx2 dy2, dx dy
```

`S` produces the same type of curve as earlier—but if it follows another `S` command or a `C` command, the first control point is assumed to be a reflection of the one used previously. If the `S` command doesn't follow another `S` or `C` command, then the current position of the cursor is used as the first control point. The result is not the same as what the `Q` command would have produced with the same parameters, but is similar.

An example of this syntax is shown below, and in the figure to the left the specified control points are shown in red, and the inferred control point in blue.

```html live-sample___shortcut_cubic_bezier
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path
    d="M 10 80 C 40 10, 65 10, 95 80 S 150 150, 180 80"
    stroke="black"
    fill="transparent" />
</svg>
```

```html hidden live-sample___shortcut_cubic_bezier
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <line x1="10" y1="80" x2="40" y2="10" stroke="red" />
    <line x1="65" y1="10" x2="95" y2="80" stroke="red" />
    <line x1="95" y1="80" x2="125" y2="150" stroke="blue" />
    <line x1="150" y1="150" x2="180" y2="80" stroke="red" />
    <circle cx="10" cy="80" r="3" fill="red" />
    <circle cx="40" cy="10" r="3" fill="red" />
    <circle cx="65" cy="10" r="3" fill="red" />
    <circle cx="95" cy="80" r="3" fill="red" />
    <circle cx="125" cy="150" r="3" fill="blue" />
    <circle cx="150" cy="150" r="3" fill="red" />
    <circle cx="180" cy="80" r="3" fill="red" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___shortcut_cubic_bezier
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('shortcut_cubic_bezier', 190, 190) }}

The other type of Bézier curve, the quadratic curve called with `Q`, is actually a simpler curve than the cubic one. It requires one control point which determines the slope of the curve at both the start point and the end point. It takes two parameters: the control point and the end point of the curve.

> [!NOTE]
> The co-ordinate deltas for `q` are both relative to the previous point (that is, `dx` and `dy` are not relative to `dx1` and `dy1`).

```plain
Q x1 y1, x y
(or)
q dx1 dy1, dx dy
```

```html live-sample___quadratic_bezier
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path d="M 10 80 Q 95 10 180 80" stroke="black" fill="transparent" />
</svg>
```

```html hidden live-sample___quadratic_bezier
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <line x1="10" y1="80" x2="95" y2="10" stroke="red" />
    <line x1="95" y1="10" x2="180" y2="80" stroke="red" />
    <circle cx="10" cy="80" r="3" fill="red" />
    <circle cx="180" cy="80" r="3" fill="red" />
    <circle cx="95" cy="10" r="3" fill="red" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___quadratic_bezier
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('quadratic_bezier', 190, 190) }}

As with the cubic Bézier curve, there is a shortcut for stringing together multiple quadratic Béziers, called with `T`.

```plain
T x y
(or)
t dx dy
```

This shortcut looks at the previous control point used and infers a new one from it. This means that after the first control point, fairly complex shapes can be made by specifying only end points.

This only works if the previous command was a `Q` or a `T` command. If not, then the control point is assumed to be the same as the previous point, and only lines will be drawn.

```html live-sample___shortcut_quadratic_bezier
<svg width="190" height="160" xmlns="http://www.w3.org/2000/svg">
  <path
    d="M 10 80 Q 52.5 10, 95 80 T 180 80"
    stroke="black"
    fill="transparent" />
</svg>
```

```html hidden live-sample___shortcut_quadratic_bezier
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <line x1="10" y1="80" x2="52.5" y2="10" stroke="red" />
    <line x1="52.5" y1="10" x2="95" y2="80" stroke="red" />
    <line x1="95" y1="80" x2="137.5" y2="150" stroke="blue" />
    <line x1="137.5" y1="150" x2="180" y2="80" stroke="blue" />
    <circle cx="10" cy="80" r="3" fill="red" />
    <circle cx="52.5" cy="10" r="3" fill="red" />
    <circle cx="95" cy="80" r="3" fill="red" />
    <circle cx="137.5" cy="150" r="3" fill="blue" />
    <circle cx="180" cy="80" r="3" fill="blue" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___shortcut_quadratic_bezier
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('shortcut_quadratic_bezier', 190, 190) }}

Both curves produce similar results, although the cubic one allows greater freedom in exactly what the curve looks like. Deciding which curve to use is situational and depends on the amount of symmetry the line has.

### Arcs

The other type of curved line that can be created using SVG is the arc, called with the `A` command. Arcs are sections of circles or ellipses.

For a given x-radius and y-radius, there are two ellipses that can connect any two points (as long as they're within the radius of the circle). Along either of those circles, there are two possible paths that can be taken to connect the points—so in any situation, there are four possible arcs available.

Because of that, arcs require quite a few parameters:

```plain
A rx ry x-axis-rotation large-arc-flag sweep-flag x y
a rx ry x-axis-rotation large-arc-flag sweep-flag dx dy
```

At its start, the arc element takes in two parameters for the x-radius and y-radius. If needed, see {{SVGElement("ellipse")}}s and how they behave. The final two parameters designate the x and y coordinates to end the stroke. Together, these four values define the basic structure of the arc.

The third parameter describes the rotation of the arc. This is best explained with an example:

```html live-sample___arcs_axis_rotation
<svg width="320" height="320" xmlns="http://www.w3.org/2000/svg">
  <path
    d="M 10 315
       L 110 215
       A 30 50 0 0 1 162.55 162.45
       L 172.55 152.45
       A 30 50 -45 0 1 215.1 109.9
       L 315 10"
    stroke="black"
    fill="green"
    stroke-width="2"
    fill-opacity="0.5" />
</svg>
```

```html hidden live-sample___arcs_axis_rotation
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <ellipse
      cx="136.225"
      cy="188.275"
      rx="30"
      ry="50"
      stroke="red"
      fill="none" />
    <ellipse
      cx="193.5"
      cy="131.5"
      rx="30"
      ry="50"
      stroke="red"
      fill="none"
      transform="rotate(-45)"
      transform-origin="193.5 131.5" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___arcs_axis_rotation
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('arcs_axis_rotation', 320, 350) }}

The example shows a `<path>` element that goes diagonally across the page. At its center, two elliptical arcs have been cut out (x radius = `30`, y radius = `50`). In the first one, the x-axis-rotation has been left at `0`, so the ellipse that the arc travels around (shown in gray) is oriented straight up and down. For the second arc, though, the x-axis-rotation is set to `-45` degrees. This rotates the ellipse so that it is aligned with its minor axis along the path direction, as shown by the second ellipse in the example image.

For the unrotated ellipse in the image above, there are only two different arcs and not four to choose from because the line drawn from the start and end of the arc goes through the center of the ellipse. In a slightly modified example the two ellipses that form the four different arcs can be seen:

```html live-sample___arcs_axis_rotation_2
<svg xmlns="http://www.w3.org/2000/svg" width="320" height="320">
  <path
    d="M 10 315
       L 110 215
       A 36 60 0 0 1 150.71 170.29
       L 172.55 152.45
       A 30 50 -45 0 1 215.1 109.9
       L 315 10"
    stroke="black"
    fill="green"
    stroke-width="2"
    fill-opacity="0.5" />
</svg>
```

```html hidden live-sample___arcs_axis_rotation_2
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <circle cx="150.71" cy="170.29" r="3" fill="red" />
    <circle cx="110" cy="215" r="3" fill="red" />
    <ellipse
      cx="144.931"
      cy="229.512"
      rx="36"
      ry="60"
      fill="transparent"
      stroke="red" />
    <ellipse
      cx="115.779"
      cy="155.778"
      rx="36"
      ry="60"
      fill="transparent"
      stroke="red" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___arcs_axis_rotation_2
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('arcs_axis_rotation_2', 320, 350) }}

Notice that each of the blue ellipses are formed by two arcs, depending on traveling clockwise or counter-clockwise. Each ellipse has one short arc and one long arc. The two ellipses are just mirror images of each other. They are flipped along the line formed from the start→end points.

If the start→end points are farther than the ellipse's `x` and `y` radius can reach, the ellipse's radii will be minimally expanded so it could reach the start→end points. The interactive codepen at the bottom of this page demonstrates this well. To determine if an ellipse's radii are large enough to require expanding, a system of equations would need to be solved, such as [this on wolfram alpha](<https://www.wolframalpha.com/input/?i=solve+((110+-+x)%5E2%2F36%5E2)+%2B+((215+-+y)%5E2%2F60%5E2)+%3D+1,+((150.71+-+x)%5E2%2F36%5E2)+%2B+((170.29+-+y)%5E2%2F60%5E2)+%3D+1>). This computation is for the non-rotated ellipse with start→end (`110`, `215`)→(`150.71`, `170.29`). The solution, (`x`, `y`), is the center of the ellipse(s). The solution will be [imaginary](<https://www.wolframalpha.com/input/?i=solve+((110+-+x)%5E2%2F30%5E2)+%2B+((215+-+y)%5E2%2F50%5E2)+%3D+1,+((162.55+-+x)%5E2%2F30%5E2)+%2B+((162.45+-+y)%5E2%2F50%5E2)+%3D+1>) if the ellipse's radii are too small. This second computation is for the non-rotated ellipse with start→end (`110`, `215`)→(`162.55`, `162.45`). The solution has a small imaginary component because the ellipse was just barely expanded.

The four different paths mentioned above are determined by the next two parameter flags. As mentioned earlier, there are still two possible ellipses for the path to travel around and two different possible paths on both ellipses, giving four possible paths. The first parameter is the `large-arc-flag`. It determines if the arc should be greater than or less than 180 degrees; in the end, this flag determines which direction the arc will travel around a given circle. The second parameter is the `sweep-flag`. It determines if the arc should begin moving at positive angles or negative ones, which essentially picks which of the two circles will be traveled around. The example below shows all four possible combinations, along with the two circles for each case.

```html live-sample___arcs_flags
<svg width="360" height="360" xmlns="http://www.w3.org/2000/svg">
  <path
    d="M 100 100
       A 45 45, 0, 0, 0, 145 145
       L 145 100 Z"
    fill="#00FF00A0"
    stroke="black"
    stroke-width="2" />
  <path
    d="M 250 100
       A 45 45, 0, 1, 0, 295 145
       L 295 100 Z"
    fill="#FF0000A0"
    stroke="black"
    stroke-width="2" />
  <path
    d="M 100 250
       A 45 45, 0, 0, 1, 145 295
       L 145 250 Z"
    fill="#FF00FFA0"
    stroke="black"
    stroke-width="2" />
  <path
    d="M 250 250
       A 45 45, 0, 1, 1, 295 295
       L 295 250 Z"
    fill="#0000FFA0"
    stroke="black"
    stroke-width="2" />
  <path
    d="M 45 45 L 345 45 L 345 345 L 45 345 Z M 195 45 L 195 345 M 45 195 L 345 195"
    fill="none"
    stroke="black" />
  <text x="140" y="20" font-size="20" fill="black">Large arc flag</text>
  <text
    x="-15"
    y="195"
    font-size="20"
    fill="black"
    transform="rotate(-90)"
    transform-origin="20 195">
    Sweep flag
  </text>
  <text x="120" y="40" font-size="20" fill="black">0</text>
  <text x="270" y="40" font-size="20" fill="black">1</text>
  <text x="30" y="120" font-size="20" fill="black">0</text>
  <text x="30" y="270" font-size="20" fill="black">1</text>
</svg>
```

```html hidden live-sample___arcs_flags
<svg style="display:none" xmlns="http://www.w3.org/2000/svg">
  <g id="reference">
    <circle cx="145" cy="100" r="45" stroke="#888888E0" fill="none" />
    <circle cx="100" cy="145" r="45" stroke="#888888E0" fill="none" />
    <circle cx="295" cy="100" r="45" stroke="#888888E0" fill="none" />
    <circle cx="250" cy="145" r="45" stroke="#888888E0" fill="none" />
    <circle cx="145" cy="250" r="45" stroke="#888888E0" fill="none" />
    <circle cx="100" cy="295" r="45" stroke="#888888E0" fill="none" />
    <circle cx="295" cy="250" r="45" stroke="#888888E0" fill="none" />
    <circle cx="250" cy="295" r="45" stroke="#888888E0" fill="none" />
  </g>
</svg>
<button>Show/hide reference points and lines</button>
```

```js hidden live-sample___arcs_flags
const g = document.querySelector("#reference");
const svg = document.querySelector("svg");
const button = document.querySelector("button");
let isHidden = true;
button.addEventListener("click", () => {
  isHidden = !isHidden;
  if (isHidden) {
    svg.querySelector("#reference").remove();
  } else {
    svg.appendChild(g.cloneNode(true));
  }
});
```

{{ EmbedLiveSample('arcs_flags', 360, 390) }}

Arcs are an easy way to create pieces of circles or ellipses in drawings. For instance, a pie chart would require a different arc for each piece.

If transitioning to SVG from {{HTMLElement("canvas")}}, arcs can be the hardest thing to learn, but are also much more powerful. Complete circles and ellipses are the only shapes that SVG arcs have trouble drawing. Because the start and end points for any path going around a circle are the same point, there are an infinite number of circles that could be chosen, and the actual path is undefined. It's possible to approximate them by making the start and end points of the path slightly askew, and then connecting them with another path segment. For example, it's possible to make a circle with an arc for each semi-circle. At that point, it's often easier to use a real {{SVGElement("circle")}} or {{SVGElement("ellipse")}} node instead. This interactive demo might help understand the concepts behind SVG arcs.

```html hidden live-sample___arcs_interactive
<script src="https://cdn.rawgit.com/lingtalfi/simpledrag/master/simpledrag.js"></script>
<div class="ui">
  <div class="controls">
    Radius X: <input id="rx" type="range" min="0" max="500" /><br />
    Radius Y: <input id="ry" type="range" min="0" max="500" /><br />
    Rotation:
    <input id="rot" type="range" min="0" max="360" value="0" /><br />
    Large arc flag: <input id="laf" type="checkbox" /><br />
    Sweep flag: <input id="sf" type="checkbox" /><br />
    Arc command: <span id="arc-value"></span><br />
  </div>
  <div class="results">
    mouse: pageX <span id="page-x"></span>, pageY <span id="page-y"></span
    ><br />
    A: <span id="ax-value"></span>, <span id="ay-value"></span><br />
    B: <span id="bx-value"></span>, <span id="by-value"></span><br />
    m: <span id="m-value"></span><br />
    b(A): <span id="ba-value"></span><br />
    b(B): <span id="bb-value"></span><br />
    contextWidth: <span id="cw-value"></span><br />
  </div>
</div>

<svg width="100%" height="100%" id="svg-context">
  <path id="arc2" d="" fill="none" stroke="green" stroke-width="2"></path>
  <path id="arc3" d="" fill="none" stroke="green" stroke-width="2"></path>
  <path id="arc4" d="" fill="none" stroke="green" stroke-width="2"></path>

  <path
    id="arc"
    d="M100 100 A 100 100 0 1 0 200 100"
    fill="none"
    stroke="red"
    stroke-width="4"></path>

  <line
    id="line0"
    x1="0"
    y1="0"
    x2="0"
    y2="0"
    fill="none"
    stroke="black"
    stroke-width="2"></line>
  <line
    id="line"
    x1="0"
    y1="0"
    x2="0"
    y2="0"
    fill="none"
    stroke="black"
    stroke-width="2"></line>
  <line
    id="line2"
    x1="0"
    y1="0"
    x2="0"
    y2="0"
    fill="none"
    stroke="black"
    stroke-width="2"></line>

  <circle
    id="circle1"
    cx="100"
    cy="100"
    r="5"
    fill="red"
    stroke="red"
    stroke-width="2"></circle>

  <circle
    id="circle2"
    cx="200"
    cy="100"
    r="5"
    fill="red"
    stroke="red"
    stroke-width="2"></circle>
</svg>
```

```css hidden live-sample___arcs_interactive
body {
  position: fixed;
  width: 100%;
  height: 100%;
  background: #eee;
}

.ui {
  display: flex;
}

.ui > div {
  margin: 0 10px;
}

.ui .controls input {
  vertical-align: middle;
}

#circle1,
#circle2 {
  cursor: pointer;
}

svg {
  background: #ddd;
}
```

```js hidden live-sample___arcs_interactive
const svgContext = document.getElementById("svg-context");
let rect = svgContext.getBoundingClientRect(); // helper to enclose mouse coordinates into svg box

const pageXEl = document.getElementById("page-x");
const pageYEl = document.getElementById("page-y");
const mEl = document.getElementById("m-value");
const rxEl = document.getElementById("rx");
const ryEl = document.getElementById("ry");
const rotEl = document.getElementById("rot");
const lafEl = document.getElementById("laf");
const sfEl = document.getElementById("sf");
const axEl = document.getElementById("ax-value");
const ayEl = document.getElementById("ay-value");
const bxEl = document.getElementById("bx-value");
const byEl = document.getElementById("by-value");
const baEl = document.getElementById("ba-value");
const bbEl = document.getElementById("bb-value");
const circle1 = document.getElementById("circle1");
const circle2 = document.getElementById("circle2");
const line = document.getElementById("line");
const line0 = document.getElementById("line0");
const line2 = document.getElementById("line2");
const cwEl = document.getElementById("cw-value");
const arcCmdEl = document.getElementById("arc-value");
const arcEl = document.getElementById("arc");
const arc2El = document.getElementById("arc2");
const arc3El = document.getElementById("arc3");
const arc4El = document.getElementById("arc4");

function updatePaths(pageX, pageY) {
  pageXEl.textContent = pageX;
  pageYEl.textContent = pageY;

  // line between two points
  line.setAttribute("x1", circle1.getAttribute("cx"));
  line.setAttribute("y1", circle1.getAttribute("cy"));
  line.setAttribute("x2", circle2.getAttribute("cx"));
  line.setAttribute("y2", circle2.getAttribute("cy"));

  axEl.textContent = circle1.getAttribute("cx");
  ayEl.textContent = circle1.getAttribute("cy");
  bxEl.textContent = circle2.getAttribute("cx");
  byEl.textContent = circle2.getAttribute("cy");

  // y = mx + b
  let m, b, run; // m = rise/run = (y2-y1) / (x2-x1)
  if (circle1.getAttribute("cx") <= circle2.getAttribute("cx")) {
    run = circle2.getAttribute("cx") - circle1.getAttribute("cx");
    if (run !== 0) {
      m = (circle2.getAttribute("cy") - circle1.getAttribute("cy")) / run;
    }
  } else {
    run = circle1.getAttribute("cx") - circle2.getAttribute("cx");
    if (run !== 0) {
      m = (circle1.getAttribute("cy") - circle2.getAttribute("cy")) / run;
    }
  }

  if (run !== 0) {
    // b = y - mx
    b = circle1.getAttribute("cy") - m * circle1.getAttribute("cx");
    b2 = circle2.getAttribute("cy") - m * circle2.getAttribute("cx");
    baEl.textContent = b;
    bbEl.textContent = b2;
    mEl.textContent = m;

    // draw segment from the left vertical axis (x=0) to the left most point (A or B).
    // x=0 ----> y = b
    let leftMost, rightMost;
    if (circle1.getAttribute("cx") <= circle2.getAttribute("cx")) {
      leftMost = circle1;
      rightMost = circle2;
    } else {
      leftMost = circle2;
      rightMost = circle1;
    }

    line0.setAttribute("x1", 0);
    line0.setAttribute("y1", b);
    line0.setAttribute("x2", leftMost.getAttribute("cx"));
    line0.setAttribute("y2", leftMost.getAttribute("cy"));

    // draw segment from point B to the right vertical axis (x=rect.width)
    // representing the end of the svg box.
    // y = mx + b
    const y = m * rect.width + b;
    line2.setAttribute("x1", rightMost.getAttribute("cx"));
    line2.setAttribute("y1", rightMost.getAttribute("cy"));
    line2.setAttribute("x2", rect.width);
    line2.setAttribute("y2", y);

    // now update the arc
    const arcCmd = getArcCommand(
      leftMost,
      rightMost,
      lafEl.checked,
      sfEl.checked,
    );
    arcCmdEl.textContent = arcCmd;
    arcEl.setAttribute("d", arcCmd);

    // now update the other helper arcs
    const combo = [
      [true, true],
      [true, false],
      [false, true],
      [false, false],
    ].filter(
      (item) => !(item[0] === lafEl.checked && item[1] === sfEl.checked),
    );
    arc2El.setAttribute(
      "d",
      getArcCommand(leftMost, rightMost, combo[0][0], combo[0][1]),
    );
    arc3El.setAttribute(
      "d",
      getArcCommand(leftMost, rightMost, combo[1][0], combo[1][1]),
    );
    arc4El.setAttribute(
      "d",
      getArcCommand(leftMost, rightMost, combo[2][0], combo[2][1]),
    );
  }
}

function getArcCommand(leftMost, rightMost, lafChecked, sfChecked) {
  return `M${leftMost.getAttribute("cx")} ${leftMost.getAttribute("cy")} A ${rxEl.value} ${ryEl.value} ${rotEl.value} ${lafChecked ? "1" : "0"} ${sfChecked ? "1" : "0"} ${rightMost.getAttribute("cx")} ${rightMost.getAttribute("cy")}`;
}

function updateScreen() {
  rect = svgContext.getBoundingClientRect();
  cwEl.textContent = rect.width;
}

circle1.sdrag((el, pageX, startX, pageY, startY) => {
  pageX -= rect.left;
  pageY -= rect.top;

  el.setAttribute("cx", pageX);
  el.setAttribute("cy", pageY);
  updatePaths(pageX, pageY);
});

circle2.sdrag((el, pageX, startX, pageY, startY) => {
  pageX -= rect.left;
  pageY -= rect.top;

  el.setAttribute("cx", pageX);
  el.setAttribute("cy", pageY);
  updatePaths(pageX, pageY);
});

window.addEventListener("resize", updateScreen);

// sliders
["rx", "ry", "rot"].forEach((id) => {
  document.getElementById(id).addEventListener("input", (e) => {
    updatePaths();
  });
});

// checkboxes
["laf", "sf"].forEach((id) => {
  document.getElementById(id).addEventListener("change", (e) => {
    updatePaths();
  });
});

updatePaths();
updateScreen();
```

{{EmbedLiveSample("arcs_interactive", "", 600)}}

{{ PreviousNext("Web/SVG/Tutorials/SVG_from_scratch/Basic_shapes", "Web/SVG/Tutorials/SVG_from_scratch/Fills_and_strokes") }}
