import pygame
import random

# Pygame initialisieren
pygame.init()

# Bildschirmgröße
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Autospiel")

# Farben
WHITE = (255, 255, 255)
RED = (200, 0, 0)
BLUE = (0, 0, 255)

# Auto laden
car_width, car_height = 50, 80
car = pygame.image.load("car.png")  # Stelle sicher, dass du eine Datei "car.png" hast
car = pygame.transform.scale(car, (car_width, car_height))
car_x, car_y = WIDTH//2 - car_width//2, HEIGHT - 120

# Hindernisse
obstacle_width, obstacle_height = 50, 80
obstacle_x = random.randint(0, WIDTH - obstacle_width)
obstacle_y = -obstacle_height
obstacle_speed = 5

# Geschwindigkeit
car_speed = 5

# Spiel-Loop
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(WHITE)
    
    # Events abfragen
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Steuerung
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and car_x > 0:
        car_x -= car_speed
    if keys[pygame.K_RIGHT] and car_x < WIDTH - car_width:
        car_x += car_speed

    # Hindernis bewegen
    obstacle_y += obstacle_speed
    if obstacle_y > HEIGHT:
        obstacle_y = -obstacle_height
        obstacle_x = random.randint(0, WIDTH - obstacle_width)

    # Kollisionserkennung
    if (car_x < obstacle_x + obstacle_width and car_x + car_width > obstacle_x and
        car_y < obstacle_y + obstacle_height and car_y + car_height > obstacle_y):
        print("Game Over!")
        running = False

    # Zeichnen
    pygame.draw.rect(screen, RED, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))
    screen.blit(car, (car_x, car_y))

    pygame.display.update()
    clock.tick(30)

pygame.quit()Man
