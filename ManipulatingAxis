from PySide2.QtCore import *
from PySide2.QtGui import *
from PySide2.QtWidgets import *
import hou as hou


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        if not MainWindow.objectName():
            MainWindow.setObjectName(u"MainWindow")
        MainWindow.resize(300, 200)
        self.centralwidget = QWidget(MainWindow)
        self.centralwidget.setObjectName(u"centralwidget")
        self.horizontalLayout = QHBoxLayout(self.centralwidget)
        self.horizontalLayout.setObjectName(u"horizontalLayout")
        self.verticalLayout = QVBoxLayout()
        self.verticalLayout.setObjectName(u"verticalLayout")
        self.horizontalLayout_2 = QHBoxLayout()
        self.horizontalLayout_2.setObjectName(u"horizontalLayout_2")
        self.BUT_NEW = QRadioButton(self.centralwidget)
        self.BUT_NEW.setObjectName(u"BUT_NEW")
        self.BUT_NEW.setAutoFillBackground(False)
        self.BUT_NEW.setChecked(True)

        self.horizontalLayout_2.addWidget(self.BUT_NEW)

        self.BUT_ORI = QRadioButton(self.centralwidget)
        self.BUT_ORI.setObjectName(u"BUT_ORI")

        self.horizontalLayout_2.addWidget(self.BUT_ORI)


        self.verticalLayout.addLayout(self.horizontalLayout_2)

        self.gridLayout = QGridLayout()
        self.gridLayout.setObjectName(u"gridLayout")
        self.PB_UP = QPushButton(self.centralwidget)
        self.PB_UP.setObjectName(u"PB_UP")
        self.PB_UP.setEnabled(True)
        self.PB_UP.clicked.connect(self.UpAxis)

        self.gridLayout.addWidget(self.PB_UP, 0, 0, 1, 1)

        self.PB_LEFT = QPushButton(self.centralwidget)
        self.PB_LEFT.setObjectName(u"PB_LEFT")
        self.PB_LEFT.clicked.connect(self.LeftAxis)

        self.gridLayout.addWidget(self.PB_LEFT, 0, 1, 1, 1)

        self.PB_FRONT = QPushButton(self.centralwidget)
        self.PB_FRONT.setObjectName(u"PB_FRONT")
        self.PB_FRONT.clicked.connect(self.FrontAxis)

        self.gridLayout.addWidget(self.PB_FRONT, 0, 2, 1, 1)

        self.PB_DOWN = QPushButton(self.centralwidget)
        self.PB_DOWN.setObjectName(u"PB_DOWN")
        self.PB_DOWN.clicked.connect(self.DownAxis)

        self.gridLayout.addWidget(self.PB_DOWN, 1, 0, 1, 1)

        self.PB_RIGHT = QPushButton(self.centralwidget)
        self.PB_RIGHT.setObjectName(u"PB_RIGHT")
        self.PB_RIGHT.clicked.connect(self.RightAxis)

        self.gridLayout.addWidget(self.PB_RIGHT, 1, 1, 1, 1)

        self.PB_BACK = QPushButton(self.centralwidget)
        self.PB_BACK.setObjectName(u"PB_BACK")
        self.PB_BACK.clicked.connect(self.BackAxis)

        self.gridLayout.addWidget(self.PB_BACK, 1, 2, 1, 1)


        self.verticalLayout.addLayout(self.gridLayout)

        self.PB_CENTER = QPushButton(self.centralwidget)
        self.PB_CENTER.setObjectName(u"PB_CENTER")
        self.PB_CENTER.clicked.connect(self.CentAxis)

        self.verticalLayout.addWidget(self.PB_CENTER)


        self.horizontalLayout.addLayout(self.verticalLayout)

        MainWindow.setCentralWidget(self.centralwidget)

        self.retranslateUi(MainWindow)

        QMetaObject.connectSlotsByName(MainWindow)
    # setupUi

    def retranslateUi(self, MainWindow):
        MainWindow.setWindowTitle(QCoreApplication.translate("MainWindow", u"Manipulate Axis", None))
        self.BUT_NEW.setText(QCoreApplication.translate("MainWindow", u"Create New XFrom", None))
        self.BUT_ORI.setText(QCoreApplication.translate("MainWindow", u"Work on Original XForm", None))
        self.PB_UP.setText(QCoreApplication.translate("MainWindow", u"UP", None))
        self.PB_LEFT.setText(QCoreApplication.translate("MainWindow", u"LEFT", None))
        self.PB_FRONT.setText(QCoreApplication.translate("MainWindow", u"FRONT", None))
        self.PB_DOWN.setText(QCoreApplication.translate("MainWindow", u"DOWN", None))
        self.PB_RIGHT.setText(QCoreApplication.translate("MainWindow", u"RIGHT", None))
        self.PB_BACK.setText(QCoreApplication.translate("MainWindow", u"BACK", None))
        self.PB_CENTER.setText(QCoreApplication.translate("MainWindow", u"CENTER", None))
    # retranslateUi

# ^^ PySide2 UI
#--------------------------------------------------------------------------------------------------------------------
    def showErrorN(self):
        QMessageBox.about(self.centralwidget, "Horrible Error", "Select the target nodes first!!!")

    def showErrorO(self):
        QMessageBox.about(self.centralwidget, "Horrible Error", "Select the transform nodes first!!!")

    def CentAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'centerAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()

                    transformNode.parm("movecentroid").pressButton()
                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})
            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    tx = transformNode.parm("tx").eval()
                    ty = transformNode.parm("ty").eval()
                    tz = transformNode.parm("tz").eval()

                    transformNode.parm("movecentroid").pressButton()
                    transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})


    def LeftAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'leftAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    px = transformNode.parm("px").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'px': px + bbox[0],})

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:

                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        px = transformNode.parm("px").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'px': px + bbox[0], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()

    def RightAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'rightAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    px = transformNode.parm("px").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'px': px + bbox[0],})

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        px = transformNode.parm("px").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'px': px + bbox[0], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()


    def FrontAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'frontAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    pz = transformNode.parm("pz").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'pz': pz + bbox[2], })

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        pz = transformNode.parm("pz").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'pz': pz + bbox[2], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()

    def BackAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'backAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    pz = transformNode.parm("pz").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'pz': pz + bbox[2], })

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        pz = transformNode.parm("pz").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'pz': pz + bbox[2], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()

    def UpAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'upAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    py = transformNode.parm("py").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'py': py + bbox[1], })

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        py = transformNode.parm("py").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().maxvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'py': py + bbox[1], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()

    def DownAxis(self):
        selectedNode = hou.selectedNodes()

        if len(selectedNode) is 0:
            self.showErrorN()

        else:
            if self.BUT_NEW.isChecked() is True:
                for node in selectedNode:
                    transformNode = node.parent().createNode('xform', 'bottomAxis')
                    hou.node(transformNode.path()).setInput(0, hou.node(node.path()))
                    transformNode.moveToGoodPosition()
                    transformNode.parm("movecentroid").pressButton()
                    #  ^^  1. move the obj to the world center

                    py = transformNode.parm("py").eval()
                    #  ^^  2. get the transformation value
                    bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                    #  ^^  3. get the bbox value at world center

                    transformNode.setParms({'py': py + bbox[1], })

                    transformNode.setParms({'tx': 0, 'ty': 0, 'tz': 0})

            else:
                for node in selectedNode:
                    transformNode = hou.node(node.path())

                    if transformNode.type().name() is not "xform":
                        tx = transformNode.parm("tx").eval()
                        ty = transformNode.parm("ty").eval()
                        tz = transformNode.parm("tz").eval()
                        #  ^^  1. store the original xform

                        transformNode.parm("movecentroid").pressButton()
                        #  ^^  2. move the obj to the world center

                        py = transformNode.parm("py").eval()
                        #  ^^  3. get the transformation value
                        bbox = hou.node(transformNode.path()).geometry().boundingBox().minvec()
                        #  ^^  4. get the bbox value at world center

                        transformNode.setParms({'py': py + bbox[1], })

                        transformNode.setParms({'tx': tx, 'ty': ty, 'tz': tz})

                    else:
                        self.showErrorO()

# ^^ Houdini Function
# --------------------------------------------------------------------------------------------------------------------


class MainWindow(QMainWindow):

    def __init__(self):
        QMainWindow.__init__(self)

        self.ui = Ui_MainWindow()
        self.ui.setupUi(self)



mainw = MainWindow()
mainw.show()
