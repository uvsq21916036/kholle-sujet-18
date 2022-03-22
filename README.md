# kholle-sujet-18
from base64 import encode
from pickle import STOP
import tkinter as tk
import random
from tracemalloc import stop
from xml.dom.minidom import ReadOnlySequentialNamedNodeMap

##################
# Constantes

LARGEUR = 600
HAUTEUR = 400
rayon = 20
c = 0

###################
# Fonctions

def creer_balle():
    """Dessine un disque bleu et retourne son identifiant
     et les valeurs de déplacements dans une liste"""
    global rayon
    x, y = LARGEUR // 2, HAUTEUR // 2
    dx, dy = 3, 5
    cercle = canvas.create_oval((x-rayon, y-rayon),
                                (x+rayon, y+rayon),
                                fill="blue")
    return [cercle, dx, dy]


def mouvement():
    """Déplace la balle et ré-appelle la fonction avec un compte-à-rebours"""
    n = random.randint(0,100)
    if n > 5:
        rebond()
        canvas.move(balle[0], balle[1], balle[2])
        canvas.after(20, mouvement)
    else :
        canvas.bind('<Button-1>', clic)
        #rebond()
        #canvas.move(balle[0], balle[1], balle[2])
        #canvas.after(10, mouvement)


def clic(event):
    x0, y0, x1, y1 = canvas.coords(balle[0])
    global x,y,rayon
    x,y = event.x, event.y
    if x0 < x < x1 and y0 < y < y1:
        rayon = rayon -5
        return rayon
    else : 
        rayon = rayon +5
        return rayon
    

def rebond():
    """Fait rebondir la balle sur les bords du canevas"""
    global balle,c
    x0, y0, x1, y1 = canvas.coords(balle[0])
    #if c > 30:
        #stop
    if x0 <= 0 or x1 >= 600:
            balle[1] = -balle[1]
    if y0 <= 0 or y1 >= 400:
            balle[2] = -balle[2]
    #c = c+1


######################
# programme principal

# création des widgets
racine = tk.Tk()
canvas = tk.Canvas(racine, bg="black", width=LARGEUR, height=HAUTEUR)
canvas.grid()

# initialisation de la balle
balle = creer_balle()

# déplacement de la balle
mouvement()



# boucle principale
racine.mainloop()
