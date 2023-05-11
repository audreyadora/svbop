# SVQuence.UI

SVQuence.UI is a comprehensive? Svelte library designed for creating rich, interactive automation UIs for music, media editing applications, automation tools or other applications doing timey stuff. It provides a suite of flexible components that let you build a customizable timeline-based interface, including an editable grid for sequencer applications, a timeline player, drag-and-drop timeline sections, and a powerful node editor.

## Components

Still in early development stages, but this is the intended direction 🙂 more to come!

### 1. AutomationTrack.svelte

This component lets you edit a line graph of points across a timeline. It maps to a variable, allowing you to automate changes in that variable across a given timespan.

The points snap to subdivisions of the y-axis, with a keypress (default: Shift) that enters  focused mode on the selected point, and allows one to select any value in the y range.  

This mode also can display a tooltip with the point's value as well as implementing your own instead. A checkerboard or striped background can be displayed, with the 2 color selectors capable of recieving css tags such as theme colors from Skeleton and Tailwind or rgb and hex values.  

It's perfect for creating volume or velocity curves in MIDI files.

### 2. Editable Grid/Sequencer

Coming soon, this component will provide a grid of editable tiles, suitable for piano roll or other sequencer applications such as color coded blocks that trigger other functions/actions/components - get creative with it! 

### 3. Timeline Player

Coming soon, this component will provide a timeline player complete with ticks along the axis and custom timeline cursors.

### 4. Timeline Drag and Drop Section

Coming soon, this component will provide drag-and-drop timeline sections with slicing and other timeline editing functions.

### 5. Node Editor

Coming soon, this powerful tool will allow users to connect predefined nodes and trigger them at points along the timeline. It will also support custom code blocks for user-defined nodes and nodes that visualize output or play audio.

All components are designed to work together seamlessly, with a core layering engine that coordinates SVG elements across components.

## Installation

_Coming soon..._

## Usage

_Coming soon..._
