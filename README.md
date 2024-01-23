# jogo-python-luiz
# Example file showing a circle moving on screen
import pygame
from random import randint
# pygame setup
pygame.init()
screen = pygame.display.set_mode((1280, 720))
clock = pygame.time.Clock()
running = True
a = 640
b = 360
dt = 0
x = 20
velocidade = 400
y = 20
random = 100
bola_x = True
bola_y = True
player1_pos = pygame.Vector2(screen.get_width() / 2, screen.get_height() / 2)
player2_pos = pygame.Vector2(screen.get_width() / 2, screen.get_height() / 2)
while running:
    # poll for events
    # pygame.QUIT event means the user clicked X to close your window
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # fill the screen with a color to wipe away anything from last frame
    screen.fill("BLACK")

    rect1 = pygame.draw.rect(screen, (147,255,125), (20, y, 10, 100))
    
    rect2 = pygame.draw.rect(screen, (255,255,255), (1250, x , 10, 100))
    
    ball = pygame.draw.circle(screen, (0,200,150), (a, b), 10)
    if bola_x == True:
        a -= velocidade * dt
    else:
        a += velocidade * dt
    if bola_y == True:
        b -= random * dt
    else:
        b += random * dt
    linha_esq = pygame.draw.line(screen, (0,0,0), (15,0), (15,720))
    linha_dir = pygame.draw.line(screen, (0,0,0), (1265,15), (1265,720))
    for i in range(20):
        linha_baixo = pygame.draw.line(screen, (255,255,255), (0,720 - i), (1280,720 - i))
        linha_cima = pygame.draw.line(screen,(255,255,255), (0,0 + i), (1280,0  + i))
    
    
    keys1 = pygame.key.get_pressed()
    if keys1[pygame.K_w]:
        y -= 400 * dt
    if keys1[pygame.K_s]:
        y += 400 * dt
    keys2 = pygame.key.get_pressed()
    if keys2[pygame.K_UP]:
        x -= 400 * dt
    if keys2[pygame.K_DOWN]:
        x += 400 * dt
    
    if pygame.Rect.colliderect(linha_baixo, rect1):
        y -= 400 * dt
    if pygame.Rect.colliderect(linha_cima, rect1):
        y += 400 * dt
    if pygame.Rect.colliderect(linha_baixo, rect2):
        x -= 400 * dt
    if pygame.Rect.colliderect(linha_cima, rect2):
        x += 400 * dt
    if pygame.Rect.colliderect(rect1, ball):
        if b >= 0:
            bola_x = False
           
        else:
            bola_x = True
        if random < -500:
            random += randint(0, 200)
        if random > 500 :
            random += randint(-200, 0)
        else:
            random += randint(-200, 200)
        velocidade += 100
    if pygame.Rect.colliderect(rect2, ball):
        if b >= 0:
            bola_x = True
           
        else:
            bola_x = False 
        if random < -500:
            random += randint(0, 200)
        if random > 500 :
            random -= randint(0, 220)
        else:
            random += randint(-200, 200)
        velocidade += 100
    if pygame.Rect.colliderect(linha_cima, ball):
        if b > 0:
            bola_y = False
           
        else:
            bola_y = True
            
        
    if pygame.Rect.colliderect(linha_baixo, ball):
        if b > 0:
            bola_y = True
           
        else:
            bola_y = False 
        
    # flip() the display to put your work on screen
    pygame.display.flip()

    # limits FPS to 60
    # dt is delta time in seconds since last frame, used for framerate-
    # independent physics.
    dt = clock.tick(60) / 1000

pygame.quit()
