import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the screen
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Pong")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Ball
ball = pygame.Rect(WIDTH/2 - 15, HEIGHT/2 - 15, 30, 30)
ball_speed_x = 7 * random.choice((1, -1))
ball_speed_y = 7 * random.choice((1, -1))

# Paddles
player = pygame.Rect(WIDTH - 20, HEIGHT/2 - 60, 10, 120)
opponent = pygame.Rect(10, HEIGHT/2 - 60, 10, 120)

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move the paddles
    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP]:
        player.y -= 7
    if keys[pygame.K_DOWN]:
        player.y += 7

    # Ball movement
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # Ball collisions
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y *= -1
    if ball.colliderect(player) or ball.colliderect(opponent):
        ball_speed_x *= -1

    # Draw everything
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, player)
    pygame.draw.rect(screen, WHITE, opponent)
    pygame.draw.ellipse(screen, WHITE, ball)
    pygame.draw.aaline(screen, WHITE, (WIDTH/2, 0), (WIDTH/2, HEIGHT))

    pygame.display.flip()

    # Frame rate
    pygame.time.Clock().tick(60)

pygame.quit()
