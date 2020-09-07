
### 3.2 'pygame' 실습 1
```python
import pygame
```
pygame묘듈을 가져옵니다.
```python
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
segment_width = 15
segment_height = 15
segment_margin = 3
x_change = segment_width + segment_margin
y_change = 0

```
전역변수로 BLACK와WHITE를 지정한 후 snake게임의 높이와 너비, 그리고 그 부분 사이의 여백을 지정합니다. <br>그후 snake의 x축으로의 변화와 y축으로의 변화(속도)를 지정합니다.
```python
class Segment(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface([segment_width, segment_height])
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

```
snake게임의 객체들의 일부분을 표현하는 Segment클래스로 객체의 이미지의 높이, 너비, 색, 그리고 객체의 사각형을 만든 후 x축, y축으로를 표현하고 있습니다.<br>
그리고 Sprite의 개념을 이해해야 이 게임을 만들 수 있는데 다른 이미지와 합성하기 위해 사용하는 이미지나 애니메이션을 말합니다. “게임 화면 내에서 움직이는 물체”라고 생각해도 됩니다.
```python
pygame.init()
screen = pygame.display.set_mode([800, 600])
pygame.display.set_caption('Snake Example')
allspriteslist = pygame.sprite.Group() 

```
pygame을 시작하고 창너비 ,높이와 표시될 프로그램이름을 지정한후 pygame.sprite.Group()을 allspriteslist로 지정하고 있습니다.<br> 이후에는 pygame.sprite.Group()의 메소드를 쓸 때 allspriteslist로 더 간편하게 쓸 수 있습니다.
```python
snake_segments = []
for i in range(15):
    x = 250 - (segment_width + segment_margin) * i
    y = 30
    segment = Segment(x, y)
    snake_segments.append(segment)
    allspriteslist.add(segment)

```
 snake_segments라는 빈 배열을 만든 후 위에서 만든 클래스를 이용하여 만든 값들을 빈 배열과 스프라이트 그룹에 추가하여 snake게임의 초기상태를 만듭니다.
```python
if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                x_change = (segment_width + segment_margin) * -1
                y_change = 0
            if event.key == pygame.K_RIGHT:
                x_change = (segment_width + segment_margin)
                y_change = 0
            if event.key == pygame.K_UP:
                x_change = 0
                y_change = (segment_height + segment_margin) * -1
            if event.key == pygame.K_DOWN:
                x_change = 0
                y_change = (segment_height + segment_margin)


```
키 누르는 것에 따라 x축으로의 그리고 y축으로의 변화가 어떻게 변하는지 설정합니다.
```python
   old_segment = snake_segments.pop()
   allspriteslist.remove(old_segment)
 
   x = snake_segments[0].rect.x + x_change
   y = snake_segments[0].rect.y + y_change
   segment = Segment(x, y)
   snake_segments.insert(0, segment)
   allspriteslist.add(segment)

```
 마지막 요소를 배열과 스프라이트 그룹에서 제거합니다. 그리고 새로운 요소가 어디있을지 알아낸 뒤 배열과 스프라이트 그룹에 추가합니다.
```python
    screen.fill(BLACK)
    allspriteslist.draw(screen)
    pygame.display.flip()
    clock.tick(5)
pygame.quit()

```
화면이 어두운색으로 바뀌고 스프라이트 그룹에 요소들을 다 보여준후 시간을 멈추고 pygame은 종료됩니다.<br><br>
위 프로젝트를 실행시키면 아래와 같이 누르는 방향 키에 따라 움직이는 모습을 확인할 수 있습니다.<br>
<img src="pygame_1.gif" width="600px"/><br>

### 3.2 'pygame' 실습 2


```python
import pygame
```
pygame 실습이므로, pygame모듈을 가져옵니다.
```python
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
```
전역변수로 BLACK과 WHITE를 지정합니다.


```python
class Player(pygame.sprite.Sprite):

    def __init__(self, x, y):

        super().__init__()
 
        self.image = pygame.Surface([15, 15])
        self.image.fill(BLACK)
 
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
 
        self.change_x = 0
        self.change_y = 0
```
게임의 객체들의 일부분을 표현하는 Player클래스로 객체의 이미지의 높이, 너비, 색, 그리고 객체의 사각형을 만든 후 x축, y축으로를 표현하고 속도를 0으로 초기화 하였습니다.
```python
    def changespeed(self, x, y):
        self.change_x += x
        self.change_y += y
 
    def update(self):
        self.rect.x += self.change_x
        self.rect.y += self.change_y
```
객체의 속도를 어느만큼 변화시킬지를 정하고 객체의 새로운 위치를 지정해줍니다.
```python
pygame.init()
 
screen = pygame.display.set_mode([800, 600])
 
pygame.display.set_caption('Test')

player = Player(50, 50)
all_sprites_list = pygame.sprite.Group()
all_sprites_list.add(player)
```
pygame을 시작하고 창 너비, 높이와 표시될 프로그램 이름을 지정한 후 pygame.sprite.Group()을 all_sprites_list로 지정하고 있습니다.
이후에는 pygame.sprite.Group()의 메소드를 쓸 때 all_sprites_list로 더 간편하게 쓸 수 있습니다. 그리고 all_sprites_list에 player 객체를 추가합니다.
```python
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                player.changespeed(-3, 0)
            elif event.key == pygame.K_RIGHT:
                player.changespeed(3, 0)
            elif event.key == pygame.K_UP:
                player.changespeed(0, -3)
            elif event.key == pygame.K_DOWN:
                player.changespeed(0, 3)
 
        # Reset speed when key goes up
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT:
                player.changespeed(3, 0)
            elif event.key == pygame.K_RIGHT:
                player.changespeed(-3, 0)
            elif event.key == pygame.K_UP:
                player.changespeed(0, 3)
            elif event.key == pygame.K_DOWN:
                player.changespeed(0, -3)
```
어느 방향키를 누르는지에 따라 x축, y축으로 변화가 얼만큼 일어날지 지정해줍니다.
```python
    screen.fill(WHITE)
    all_sprites_list.draw(screen)
    pygame.display.flip()
    clock.tick(60)
 pygame.quit()
```
화면이 하얀 색으로 바뀌고 스프라이트 그룹에 요소들을 다 보여준 후 시간을 멈추고 pygame은 종료됩니다.<br><br>
위 프로젝트를 실행시키면 아래와 같이 누르는 방향 키에 따라 움직이는 모습을 확인할 수 있습니다.<br>

<img src="pygame_2.gif" width="600px"/>


### 3.2 'pygame' 실습 3
```python
import pygame
import random
```
게임을 만드는데 필요한 pygame, random 라이브러리를 import 해줍니다.
```python
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)
```
게임에서 사용할 색상들을 위 코드처럼 작성하여 전역변수로 지정해줍니다.
```python
class Block(pygame.sprite.Sprite):

    def __init__(self, color):

        super().__init__()
 
        self.image = pygame.Surface([20, 15])
        self.image.fill(color)
 
        self.rect = self.image.get_rect()
```
슈팅 게임의 객체들 중 총알을 맞춰야할 블럭을 표현하는 Block 클래스로 블럭의 크기, 색, 모양을 지정합니다.

```python
class Player(pygame.sprite.Sprite):
 
    def __init__(self):

        super().__init__()
 
        self.image = pygame.Surface([20, 20])
        self.image.fill(RED)
        self.rect = self.image.get_rect()
 
    def update(self):

        pos = pygame.mouse.get_pos()
        self.rect.x = pos[0]
```
슈팅 게임의 객체들 중 사용자가 직접 마우스로 플레이 할 슈팅머신을 표현하는 player 클래스로 슈팅머신의 크기, 색, 모양을 지정하고, 마우스의 x축을 따라 이동하도록 설정해줍니다.
```python
class Bullet(pygame.sprite.Sprite):
    """ This class represents the bullet . """
    def __init__(self):
        # Call the parent class (Sprite) constructor
        super().__init__()
 
        self.image = pygame.Surface([4, 10])
        self.image.fill(BLACK)
 
        self.rect = self.image.get_rect()
 
    def update(self):
        self.rect.y -= 3
```
슈팅 게임의 객체들 중 슈팅머신에서 발사되는 총알을 표현하는 bullet 클래스로 총알의 크기, 색, 모양을 지정하고, 총알이 y축을 따라 움직이도록 속도를 설정해줍니다.
```python
pygame.init()
 
# Set the height and width of the screen
screen_width = 700
screen_height = 400
screen = pygame.display.set_mode([screen_width, screen_height])
 
# --- Sprite lists
 
# This is a list of every sprite. All blocks and the player block as well.
all_sprites_list = pygame.sprite.Group()
 
# List of each block in the game
block_list = pygame.sprite.Group()
 
# List of each bullet
bullet_list = pygame.sprite.Group()
```
위 프로젝트를 실행시키면 아래와 같이 빨간 사각형이 마우스를 따라 움직이며 클릭 시 총알이 발사되는 모습을 확인할 수 있습니다.<br>
<img src="pygame_3.gif" width="600px"/>
