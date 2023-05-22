# fireboy-watergirl-game
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
yellow = (255, 255, 0)

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

# Класс для рычагов
class Lever(GameObject):
    def __init__(self, x, y, width, height, color, gate):
        super().__init__(x, y, width, height, color)
        self.gate = gate
        self.activated = False

    def activate(self):
        self.activated = True
        self.gate.open()

# Класс для ворот
class Gate(GameObject):
    def __init__(self, x, y, width, height, color):
        super().__init__(x, y, width, height, color)
        self.opened = False

    def open(self):
        self.opened = True

    def close(self):
        self.opened = False

# Класс для опасных жидкостей
class Hazard(GameObject):
    def __init__(self, x, y, width, height, color):
        super().__init__(x, y, width, height, color)

    def collide(self, player):
        if self.rect.colliderect(player.rect):
            player.take_damage()

# Класс для игрока
class Player(GameObject):
    def __init__(self, x, y, width, height, color):
        super().__init__(x, y, width, height, color)
        self.health = 100

    def take_damage(self):
        self.health -= 10

# Создание игровых объектов
fireboy = Player(100, 100, 50, 50, red)
watergirl = Player(200, 200, 50, 50, blue)
lever = Lever(300, 300, 30, 10, white, gate)
gate = Gate(400, 400, 50, 100, yellow)
hazard = Hazard(500, 500, 50, 50, black)

# Группы спрайтов для удобства обновления и отрисовки
all_sprites = pygame.sprite.Group()
all_sprites.add(fireboy, watergirl, lever, gate, hazard)

# Создание группы для взаимодействия с рычагом
lever_group = pygame.sprite.Group()
lever_group.add(lever)

# Создание группы для взаимодействия с опасными жидкостями
hazard_group = pygame.sprite.Group()
hazard_group.add(hazard)

# Главный игровой цикл
running = True
paused = False
clock = pygame.time.Clock()
while running:
    clock.tick(60)  # Ограничение количества кадров в секунду
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                paused = not paused  # Пауза по нажатию клавиши Esc
    
    if paused:
        continue  # Пропуск обновления и отрисовки игры, если пауза активна
    
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

    # Проверка взаимодействия с рычагом
    if pygame.sprite.spritecollideany(fireboy, lever_group):
        lever.activate()

    # Проверка взаимодействия с воротами
    if gate.opened:
        if pygame.sprite.collide_rect(fireboy, gate) or pygame.sprite.collide_rect(watergirl, gate):
            gate.close()

    # Проверка взаимодействия с опасными жидкостями
    hazard_group.update()
    if pygame.sprite.spritecollideany(fireboy, hazard_group):
        fireboy.take_damage()
    if pygame.sprite.spritecollideany(watergirl, hazard_group):
        watergirl.take_damage()

    # Отрисовка игры
    screen.fill(white)
    all_sprites.draw(screen)
    pygame.display.flip()

# Завершение игры
pygame.quit()
