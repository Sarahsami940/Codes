           # \\\\\\\\\\ Space-shooter game

import pygame
import random
import math
from pygame import mixer
from pygame.constants import K_SPACE
from pygame import mixer

# Screen's display
WIDTH , HEIGHT = 900 , 700
WIN = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("First Game !")


mixer.init()
mixer.music.load("Dynamite - BTS.mp3") # any song or music of your choice
mixer.music.play(-1)

# Icon
#icon = pygame.image.load("space.png")
#pygame.display.set_icon(icon)

# Loading images
background = pygame.image.load("spacey bg.jpg")
player_img = pygame.image.load("spaceship3.png")

bullet = pygame.image.load("bullet.png")

spaceshipX = 380
spaceshipY = 480
changeX = 0

# creatng multiple aliens
alien_img = []
alienX = []
alienY = []
alienspeedx = []

for i in range(5):
    alien_img.append(pygame.image.load("alien enemy.png"))
    alienX.append(random.randint(0,836))
    alienY.append(random.randint(0,300))
    alienspeedx.append(0.5)

check = False
bulletX = spaceshipX+46
bulletY = 449

score = 0
pygame.font.init()
font= pygame.font.SysFont('Times',35,'bold','underline')
font_gameover= pygame.font.SysFont('Helvetica',45,'bold','underline')



def scoreboard():
     score_img = font.render(f'Score: {score}',True,'White')
     WIN.blit(score_img,(10,10))

def gameover():
     gameover_img = font_gameover.render(f'GAME OVER !!',True,'White')
     WIN.blit(gameover_img,(300,250))

def collision():
    
    if distance < 27:
        return True

# Main program and loop used for game
run = True
while run :
       
    WIN.blit(background,(0,0))   # BACKGROUND IMAGE
    

    for event in pygame.event.get():

        
        if event.type == pygame.QUIT :
            run = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                changeX = -1
            if event.key == pygame.K_RIGHT:
                changeX = 1

            if event.key == K_SPACE:
                if check is False:
                   shoot = mixer.Sound("laser fire.mp3")
                   shoot.play()
                   check = True
                   bulletX = spaceshipX+46
        if event.type == pygame.KEYUP:
            changeX = 0

# control movement of spaxeship

    if spaceshipX <= 0:
        spaceshipX=0
    elif spaceshipX >= 780:
        spaceshipX=780
    spaceshipX+=changeX
                                                                                                                

# Creating multiple alien's functionality     

    for i in range(5):
         # disappearin the alien :                             
         if alienY[i] > 400 :
             for j in range(5):
                alienY[j] = 3000
             gameover()
             break

         alienX[i] += alienspeedx[i]
         if alienX[i] <= 0 :
            alienspeedx[i] = 0.5
            alienY[i] += 40
         elif alienX[i] >= 836 :
            alienspeedx[i] = -0.5
            alienY[i] += 40

        # to destoy alien with the bullet
         distance = math.sqrt(math.pow(bulletX-alienX[i],2)+math.pow(bulletY-alienY[i],2))
         if distance < 20:
             bulletY = 440
             check = False
             alienX[i] = 0
             alienY[i] = 0
             score += 1 

         WIN.blit(alien_img[i],(alienX[i], alienY[i]))      # ALIEN IMAGE

# controlling bullet movement:
    if bulletY <=0:
        bulletY = 440
        check = False

# Blitting bullet if condition is true:
    if check is True:
        WIN.blit(bullet,(bulletX, bulletY))
        bulletY-=3

# blitting images on screen
    WIN.blit(player_img,(spaceshipX ,spaceshipY))     # SPACESHIP IMAGE
    
    scoreboard()
    pygame.display.update()
pygame.quit()
