import pygame
import random

pygame.init()

pygame.display.set_caption('Snake Game')

SCREEN_WIDTH = 1200
SCREEN_HEIGHT = 600

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

BLOCK_SIZE = 20
SNAKE_SIZE = 20

screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 30)
def draw_snake(snake_list):
    for segment in snake_list:
        pygame.draw.rect(screen, GREEN, [segment[0], segment[1], SNAKE_SIZE, SNAKE_SIZE])
def game_loop():
    #Posição inicial da cobrinha
    snake_list = [[SCREEN_WIDTH / 2, SCREEN_HEIGHT / 2]]
    snake_length = 1

   
    food_x = round(random.randrange(0, SCREEN_WIDTH - BLOCK_SIZE) / BLOCK_SIZE) * BLOCK_SIZE
    food_y = round(random.randrange(0, SCREEN_HEIGHT - BLOCK_SIZE) / BLOCK_SIZE) * BLOCK_SIZE

    delta_x = 0
    delta_y = 20

    game_over = False

    while not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    delta_x = -BLOCK_SIZE
                    delta_y = 0
                elif event.key == pygame.K_RIGHT:
                    delta_x = BLOCK_SIZE
                    delta_y = 0
                elif event.key == pygame.K_UP:
                    delta_x = 0
                    delta_y = -BLOCK_SIZE
                elif event.key == pygame.K_DOWN:
                    delta_x = 0
                    delta_y = BLOCK_SIZE

        snake_head = [snake_list[-1][0] + delta_x, snake_list[-1][1] + delta_y]
        snake_list.append(snake_head)
        
        if (snake_head[0] >= SCREEN_WIDTH or snake_head[0] < 0 or
            snake_head[1] >= SCREEN_HEIGHT or snake_head[1] < 0):
            game_over = True

        for segment in snake_list[:-1]:
            if segment == snake_head:
                game_over = True

        if snake_head[0] == food_x and snake_head[1] == food_y:
            food_x = round(random.randrange(0, SCREEN_WIDTH) / BLOCK_SIZE) * BLOCK_SIZE
            food_y = round(random.randrange(0, SCREEN_HEIGHT) / BLOCK_SIZE) * BLOCK_SIZE
            snake_length += 1
        else:
            snake_list.pop(0)

        screen.fill(BLACK)

        draw_snake(snake_list)

        pygame.draw.rect(screen, RED, [food_x, food_y, BLOCK_SIZE, BLOCK_SIZE])

        pygame.display.update()

        clock.tick(10)

    pygame.quit()
    quit()

game_loop()
