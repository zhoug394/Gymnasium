import pygame
import random
import sys

# 初始化 pygame
pygame.init()

# 視窗設定
WIDTH, HEIGHT = 400, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Atari Dodge Game")

# 顏色
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# 時鐘
clock = pygame.time.Clock()
FPS = 60

# 玩家設定
player_width = 40
player_height = 20
player_x = WIDTH // 2 - player_width // 2
player_y = HEIGHT - 60
player_speed = 5

# 隕石設定
enemy_width = 40
enemy_height = 40
enemy_x = random.randint(0, WIDTH - enemy_width)
enemy_y = -enemy_height
enemy_speed = 5

# 分數
score = 0
font = pygame.font.SysFont(None, 36)

def draw_score():
    text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(text, (10, 10))

# 主遊戲迴圈
running = True
while running:
    clock.tick(FPS)
    screen.fill(BLACK)

    # 事件處理
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # 鍵盤控制
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
        player_x += player_speed

    # 隕石移動
    enemy_y += enemy_speed
    if enemy_y > HEIGHT:
        enemy_y = -enemy_height
        enemy_x = random.randint(0, WIDTH - enemy_width)
        score += 1
        enemy_speed += 0.2  # 越來越快（增加難度）

    # 碰撞判斷
    player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
    enemy_rect = pygame.Rect(enemy_x, enemy_y, enemy_width, enemy_height)

    if player_rect.colliderect(enemy_rect):
        running = False  # 遊戲結束

    # 繪圖
    pygame.draw.rect(screen, WHITE, player_rect)
    pygame.draw.rect(screen, RED, enemy_rect)
    draw_score()

    pygame.display.update()

# Game Over 畫面
screen.fill(BLACK)
game_over_text = font.render("GAME OVER", True, WHITE)
final_score_text = font.render(f"Final Score: {score}", True, WHITE)

screen.blit(game_over_text, (WIDTH//2 - 90, HEIGHT//2 - 40))
screen.blit(final_score_text, (WIDTH//2 - 110, HEIGHT//2))

pygame.display.update()
pygame.time.wait(3000)

pygame.quit()
