# Milagros-Ricter
Bloc de notas en python
from PyQt5.QtWidgets import *
from PyQt5.QtPrintSupport import *
import sys
import os

from PyQt5.sip import delete

#parametros de la ventana
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.path=None
        self.ancho = 800
        self.alto = 500
        self.posy = 100
        self.posx = 100
        self.bloc = "Mi bloc de notas"
        

    

        self.texto = QPlainTextEdit()
        self.menu  = self.menuBar()
    

        self.initUI()
    def initUI(self):
        self.v_formato = QVBoxLayout()

        #se definen los menu de la barra de tareas
        self.menu_archivo = self.menu.addMenu("Archivo")
        self.menu_editar = self.menu.addMenu("editar")
        self.menu_salir = self.menu.addMenu("Salir")


        #acciones de los botones
        self.menu_archivo_abrir = QAction("abrir...", self)
        self.menu_archivo_abrir.triggered.connect(self.Abrir)
        
        self.menu_archivo_guardar = QAction("Guardar",self)
        self.menu_archivo_guardar.triggered.connect(self.Guardar)

        self.menu_archivo_guardar_como = QAction("Guardar como...", self)
        self.menu_archivo_guardar_como.triggered.connect(self.guardarcomo)

        self.menu_editar_copiar = QAction("Copiar", self)     
        self.menu_editar_copiar.triggered.connect(self.texto.copy)

        self.menu_editar_pegar = QAction("pegar", self)     
        self.menu_editar_pegar.triggered.connect(self.texto.paste)

        self.menu_editar_borrar = QAction("borrar", self)     
        self.menu_editar_borrar.triggered.connect(self.texto.deleteLater)
       
    
        self.menu_salir_quit = QAction ("salir", self)
        self.menu_salir_quit.triggered.connect(qApp.quit)

        #acciones que realizan en la seccion de barra de tareas (archivo, editar y salir)
        self.menu_archivo.addAction(self.menu_archivo_abrir)
        self.menu_archivo.addAction(self.menu_archivo_guardar)
        self.menu_archivo.addAction(self.menu_archivo_guardar_como)
        self.menu_editar.addAction(self.menu_editar_copiar)
        self.menu_editar.addAction(self.menu_editar_pegar)
        self.menu_editar.addAction(self.menu_editar_borrar)
        self.menu_salir.addAction(self.menu_salir_quit)
        
        #formato y medida de la ventana
        self.v_formato.addWidget(self.texto)
        self.widgetPrincipal = QWidget()
        self.widgetPrincipal.setLayout(self.v_formato)
        self.setCentralWidget(self.widgetPrincipal)

        self.setGeometry(self.posx, self.posy, self.ancho, self.alto)
        self.setWindowTitle(self.bloc)


        

        self.show()
    

    #funciones que requieren realizar acciones externas, ajenas al programa
    def Abrir(self):         
        path, _ = QFileDialog.getOpenFileName(self, "Open file", "", "Documento de Texto(*.txt)") #abrir un archivo de la computadora
  
        if path: 
            try: 
                with open(path, 'rU') as f: 
                    text = f.read() 

            except Exception as e:     
                self.dialog_critical(str(e)) 
            
            else:  
                self.path = path  

    def guardarcomo(self): 
        path, _ = QFileDialog.getSaveFileName(self, "Save file", "","Documento de Texto(*.txt)") #elegir la ruta en la que se guarda el archivo
        
        if not path: 
        
            return
        
        self._guardar_en_path(path) 

    
    #conteo  de letras en el bloc
    def count_letters(path):        

            return len(filter(lambda x: x not in " texto:", path))  
            
            self.count_letters("This is a test")  

    
    def Guardar(self):          #se guarda el archivo si ya esta creado, si no es asi lo manda en guardar como para elegir la ruta
        if self.path is None: 

            return self.guardarcomo()

        self._guardar_en_path(self.path) 







app = QApplication(sys.argv)
ventana = VentanaPrincipal()
ventana.show
app.exec()

    
