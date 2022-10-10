# Houdini Axis Manipulating Tool
This is a simple python tool to manipulate axis for a sop node.
To install it, just copy the script to a new houdini shelf tool, hit that shelf tool when you want to run the script. 
You can create a new transform node through the tool, or manipulate the axis on a existing transform node.

Houdini Pyside2 Tip: 
Do not run Pyside2 in QApplication!!!

Wrong: 
app = QApplication([])
stats = Stats()
stats.window.show()
app.exec_()

Right:
stats = Stats()
stats.window.show()
