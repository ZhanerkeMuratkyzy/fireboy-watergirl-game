# fireboy-watergirl-game
Fireboy &amp; Watergirl
import pygame

# Инициализация Pygame
pygame.init()

# Размеры экрана
screen_width = 800
screen_height = 600

# Цвета
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)
blue = (0, 0, 255)

# Создание окна игры
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Fireboy & Watergirl")

# Класс для игровых объектов
class GameObject(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, color):
        super().__init__()
        self.image = pygame.Surface((width, height))
        self.image.fill(color)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

# Создание игровых объектов
fireboy = GameObject(100, 100, 50, 50, red)
watergirl = GameObject(200, 200, 50, 50, blue)

# Главный игровой цикл
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    # Обновление игры
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        fireboy.rect.x -= 5
    if keys[pygame.K_RIGHT]:
        fireboy.rect.x += 5
    if keys[pygame.K_UP]:
        fireboy.rect.y -= 5
    if keys[pygame.K_DOWN]:
        fireboy.rect.y += 5

    if keys[pygame.K_a]:
        watergirl.rect.x -= 5
    if keys[pygame.K_d]:
        watergirl.rect.x += 5
    if keys[pygame.K_w]:
        watergirl.rect.y -= 5
    if keys[pygame.K_s]:
        watergirl.rect.y += 5

    # Отрисовка игры
    screen.fill(white)
    pygame.draw.rect(screen, fireboy.image, fireboy.rect)
    pygame.draw.rect(screen, watergirl.image, watergirl.rect)
    pygame.display.flip()

# Завершение игры
pygame.quit()
