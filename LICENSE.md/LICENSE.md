from tkinter import *

"""
Fonctions
"""

def colision(Perso, obstacle): #Fonction visant à coder le fait que quand le perso est a une certaine distance du mur il y ait collision
    if Perso['x'] >= obstacle['x'] + obstacle['longueur'] or \
       Perso['x'] + 20 <= obstacle['x'] or \
       Perso['y'] >= obstacle['y'] + obstacle['hauteur'] or \
       Perso['y'] + 20 <= obstacle['y']:
        return False #return False revient a dire qu'il n'y a pas eu de collisions avant
    return True #return True revient a dire qu'il y a eu une collision


def deplacement(X,Y): #Fonction visant à coder les déplacements du personnage
    Perso = variable['Perso']
    ax, ay = Perso['x'], Perso['y']
    Perso['x'], Perso['y'] = ax + X, ay + Y #ax et ay sont les position actuelles du Perso, X et Y sont les valeurs de dépacements qui s'appliquent au Perso
    for obstacle in variable['obstacles']:
        if colision(variable['Perso'],obstacle):
            Perso['x'], Perso['y'] = ax, ay #Fait en sorte que quand il y ait une collision, la position du Perso soit ses positions actuelles
            break

    laby.coords(Perso['identifiant'], Perso['x'], Perso['y']) # Met a jour les coordonnées du Perso dans le code


def haut(): #Déplacement vers le haut
    deplacement(0,-10)

def bas(): #Déplacement vers le bas
    deplacement(0,10)

def droite(): #Déplacement vers la droite
    deplacement(10,0)

def gauche(): #Déplacement vers la gauche
    deplacement(-10,0)


def touches(event): #Fonction associant la pression d'une touche précise à un déplacement
    global frameD, frameG
    touche = event.keysym
    if touche in ("Up", "z") : #Flèche haute et z codent pour aller vers le haut
        haut()
    elif touche in ("Left", "q"): #Flèche gauche et q codent pour aller vers la gauche
        gauche()
    elif touche in ("Down", "s"): #Flèche basse et s codent pour aller vers le bas
        bas()
    elif touche in ("Right", "d") : #Flèche droite et d codent pour aller vers la droite
        droite()

def obstacles(): #Fonction codant la position en x et y des murs pour les collisions
    obstacles = variable['obstacles'] = [] #Va dans le dictionnaire

    # dans l'ordre x, y, longueur, hauteur
    coordonnees = [(0,0,20,780),(20,0,340,20),(440,0,340,20),(760,0,20,780),(0,760,380,20),(420,760,360,20),
                   (60,20,20,60),(60,120,20,40),(60,640,80,60),(120,20,20,120),(140,60,60,20),(80,120,40,20),
                   (140,120,60,20),(240,20,20,240),(340,20,20,60),(300,60,80,20),(440,20,20,60),(420,60,80,20),
                   (540,20,20,240),(560,60,40,20),(560,240,40,20),(700,20,20,60),(640,20,20,60),(560,60,40,20),
                   (600,120,120,20),(600,180,120,20),(700,140,20,40),(260,260,40,20),(300,80,20,60),(320,120,60,20),
                   (360,140,20,20),(420,120,20,40),(440,120,60,20),(480,80,20,40),(120,180,20,40),(20,200,100,20),
                   (300,180,20,40),(320,200,160,20),(480,180,20,40),(640,200,20,60),(660,240,60,20),(560,240,40,20),
                   (20,260,40,20),(100,260,80,20),(340,260,40,20),(420,260,40,20),(500,260,20,80),(100,280,20,120),
                   (160,280,20,20),(280,280,20,60),(360,280,20,60),(420,280,20,60),(500,280,20,60),(620,300,20,100),
                   (680,300,40,100),(60,320,40,20),(220,320,20,20),(300,320,60,20),(440,320,60,20),(160,340,20,60),
                   (20,380,40,20),(120,380,40,20),(220,380,60,20),(520,380,80,20),(720,380,40,20),(320,380,20,80),
                   (460,380,20,80),(220,400,20,40),(20,440,300,20),(340,440,40,20),(220,440,40,20),(480,440,280,20),
                   (600,460,20,60),(20,500,100,40),(160,500,440,20),(660,500,100,20),(660,520,20,80),(220,560,20,100),
                   (280,560,340,20),(660,560,60,40),(60,580,20,20),(120,580,40,20),(280,580,20,140),(480,580,20,60),
                   (600,580,20,140),(120,600,20,60),(660,600,20,120),(340,620,140,20),(540,620,20,100),(60,640,60,80),
                   (140,640,80,20),(720,640,40,20),(340,680,40,80),(420,680,80,20),(160,700,120,20),(420,700,20,60),
                   (560,700,40,20),(680,700,40,20),(160,720,20,40),(440,740,60,20)]
    for x,y, longueur, hauteur in coordonnees:
        mur = {'x': x, 'y': y, 'longueur': longueur, 'hauteur': hauteur}

        mur['identifiant'] = laby.create_rectangle(x, y,
                                                   x + longueur,
                                                   y + hauteur,
                                                   fill='black')
        obstacles.append(mur)


"""
Programme
"""

variable = {}
Perso = variable['Perso'] = {'x': 390, 'y': 760, 'longueur': 20, 'hauteur': 20} #Coordonnées de base du Perso

fen = Tk()
laby = Canvas(fen,width=780,height=800,bg="white")
laby.pack(side=LEFT)
mur=PhotoImage(file="pierre.gif")
chemin=PhotoImage(file="sol.gif")
X, Y = 0, 0
fichier=open("chemin.txt",'r')
for ligne in fichier:
    for i in range(len(ligne)):
        case= ligne[i]
        if case == 'm':
            laby.create_image(X,Y,image=mur,anchor="nw")
        if case == 'c':
            laby.create_image(X,Y,image=chemin,anchor="nw")
        X=X+20
    X=10
    Y=Y+20
frameD, frameG = 1, 1
img = PhotoImage(file="HAUT 2.gif")
variable['Perso']['identifiant'] = laby.create_image(Perso['x'],
                                              Perso['y'],
                                              image=img, anchor='nw') #Permet au perso d'avoir pour une image img


obstacles()
laby.focus_set()
laby.bind("<Key>", touches)
fen.mainloop()
