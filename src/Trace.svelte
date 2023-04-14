<script>
  import P5 from "p5-svelte";
  import {
    parsePath,
    serialize,
    absolutize,
    normalize,
  } from "path-data-parser";
  const svgString =
    "M10.031 12.031l1.406-1.469c-1.563-1.531-3.156-2.313-4.719-2.313-1.344 0-2.438 0.438-3.344 1.281-0.875 0.875-1.313 1.938-1.313 3.219 0 1 0.281 1.844 0.813 2.656 0.531 0.781 1.625 1.531 3.156 2.344 1.406 0.75 2.344 1.313 2.75 1.781 0.406 0.5 0.594 1.094 0.594 1.719 0 0.75-0.344 1.406-0.938 1.969-0.594 0.531-1.344 0.844-2.219 0.844-1.25 0-2.438-0.656-3.563-1.906l-1.375 1.594c0.563 0.75 1.313 1.344 2.219 1.75s1.813 0.625 2.813 0.625c1.469 0 2.656-0.469 3.656-1.438 1-1 1.5-2.156 1.5-3.531 0-1-0.313-1.875-0.875-2.688-0.563-0.781-1.719-1.594-3.344-2.438-1.344-0.688-2.219-1.313-2.625-1.813s-0.625-1.031-0.625-1.563c0-0.625 0.281-1.188 0.781-1.656 0.5-0.438 1.094-0.688 1.813-0.688 1.125 0 2.281 0.563 3.438 1.719z";

  const segments = parsePath(svgString);
  const absoluteSegments = absolutize(segments);
  const normalizedSegments = normalize(absoluteSegments);

  const pathCoordinates = [];

  console.log(normalizedSegments);
  const sketch = (p5) => {
    function drawDots(coordinates) {
      p5.stroke(255, 0, 0); // Set the dot color to red
      p5.strokeWeight(3); // Set the dot size

      for (const point of coordinates) {
        point.x = Math.round(point.x);
        point.y = Math.round(point.y);
        point(
          (p5.displayWidth * point.x) / 180,
          (p5.displayHeight * point.y) / 180
        );
      }
    }

    function getCubicBezierPoint(p0, p1, p2, p3, t) {
      const oneMinusT = 1 - t;

      const x =
        oneMinusT ** 3 * p0.x +
        3 * oneMinusT ** 2 * t * p1.x +
        3 * oneMinusT * t ** 2 * p2.x +
        t ** 3 * p3.x;
      const y =
        oneMinusT ** 3 * p0.y +
        3 * oneMinusT ** 2 * t * p1.y +
        3 * oneMinusT * t ** 2 * p2.y +
        t ** 3 * p3.y;

      return p5.createVector(x, y);
    }

    p5.setup = () => {
      p5.createCanvas(p5.windowWidth, p5.windowHeight - 40);
    };

    p5.windowResized = () => {
      p5.resizeCanvas(p5.windowWidth, p5.windowHeight - 40);
    };

    p5.draw = () => {
      p5.background(0);

      p5.stroke(255);
      p5.strokeWeight(1);
      p5.noFill();
      p5.beginShape();

      normalizedSegments.forEach((s) => {
        let { key, data } = s;

        data = data.map((d) => {
          return d * 10;
        });
        switch (key) {
          case "M":
            p5.vertex(data[0], data[1]);
            pathCoordinates.push(p5.createVector(data[0], data[1]));
            break;
          case "L":
            p5.vertex(data[0], data[1]);
            break;
          case "C":
            const startPoint = pathCoordinates[pathCoordinates.length - 1];

            // Calculate the control points and the end point
            const cp1 = p5.createVector(data[0], data[1]);
            const cp2 = p5.createVector(data[2], data[3]);
            const endPoint = p5.createVector(data[4], data[5]);
            p5.bezierVertex(cp1.x, cp1.y, cp2.x, cp2.y, endPoint.x, endPoint.y);

            // Sample the curve and add the points to the pathCoordinates array
            const sampleCount = 100;
            for (let t = 0; t <= sampleCount; t++) {
              const tNormalized = t / sampleCount;
              const point = getCubicBezierPoint(
                startPoint,
                cp1,
                cp2,
                endPoint,
                tNormalized
              );
              pathCoordinates.push(point);
            }

            break;
          case "Z":
            p5.endShape(p5.CLOSE);
            break;
        }
      });

        drawDots(pathCoordinates);
    };
  };
</script>

<div class="bg-gray-500 text-white touch-none h-full w-screen">
  <div>
    <P5 {sketch} />
  </div>
</div>
