# CS1110DungeonGame
UVA Spring 2018 CS 1110 Final Project Repository
# neh9cg and kl6bv

# at least 3 levels and then a boss; obstacles
# enemies generated ever 'x' ticks
# shooting via
# key to unlock door to next level
# countdown timer for how long the player has to beat a level
# What files are especially applicable to our game:
    # shooting.py , two_levels.py , infinite_jumper_game_over_bots.py , collection_things.py , basic_game.py
    # basic_keys.py

#the basis  of this code is a two level code as outlined by Luther Tychonievich, who releases itinto the public domain.
# the game itself for our group will be use a much different way to change levels,
# but this will be the basis as we move forwards

import pygame
import gamebox
import random

c = gamebox.Camera(800, 600)
p = gamebox.from_color(20, 550, "red", 20, 20)
cool_down = 0
p.level = 1
obs = [
    gamebox.from_color(75,300,'darkblue',30,30),gamebox.from_color(375,50,'darkblue',30,30),gamebox.from_color(500,335,'darkblue',30,30),
    gamebox.from_color(400, 600, 'green', 1000, 50),gamebox.from_color(0, 300, 'green', 50, 1000),
    gamebox.from_color(400, 0, 'green', 1000, 50),gamebox.from_color(800, 300, 'green', 50, 1000),
    gamebox.from_color(0,100,'green',150,12),gamebox.from_color(75,150,'green',12,112),
    gamebox.from_color(75,200,'green',100,12),gamebox.from_color(125,300,'green',12,212),
    gamebox.from_color(0,406,'green',262,12),gamebox.from_color(0,500,'green',200,12),
    gamebox.from_color(400,300,'green',150,150),gamebox.from_color(100,500,'green',12,50),
    gamebox.from_color(200,600,'green',12,400),gamebox.from_color(400,0,'green',12,600),
    gamebox.from_color(350,0,'green',12,200),gamebox.from_color(375,100,'green',62,12),
    gamebox.from_color(331,200,'green',12,50),gamebox.from_color(300,175,'green',74,12),
    gamebox.from_color(125,0,'green',12,200),gamebox.from_color(200,0,'green',12,400),
    gamebox.from_color(325,300,'green',200,12),gamebox.from_color(331,375,'green',12,300),
    gamebox.from_color(230,406,'green',70,12),gamebox.from_color(300,475,'green',75,12),
    gamebox.from_color(250,530,'green',100,12),gamebox.from_color(450,600,'green',12,300),
    gamebox.from_color(425,450,'green',62,12),gamebox.from_color(475,369,'green',300,12),
    gamebox.from_color(525,450,'green',12,150),gamebox.from_color(575,519,'green',100,12),
    gamebox.from_color(700,600,'green',12,450),gamebox.from_color(650,450,'green',100,12),
    gamebox.from_color(500,300,'green',60,12),gamebox.from_color(525,344,'green',12,100),
    gamebox.from_color(650,0,'green',12,500),gamebox.from_color(660,100,'green',150,12),
    gamebox.from_color(550,0,'green',12,400),gamebox.from_color(500,100,'green',100,12),
    gamebox.from_color(800,300,'green',400,12),gamebox.from_color(800,200,'green',200,12),
]
door = [
    gamebox.from_image(675,50, "https://stefanpoag.files.wordpress.com/2015/12/door-in-process.jpg")
]
bullets1 = [
    ]
bullets2 = [
    ]
bullets3 = [
    ]
bullets4 = [
    ]
enemies = [
    ]
level = 1
def use_obstacles():
    p.move_speed()
    for o in obs:
        p.move_to_stop_overlapping(o)
    for o in door:
        o.size = [40,60]
        c.draw(o)

def level1(keys):
    global ticks
    global level
    c.clear('black')
    use_obstacles()
    for o in door:
        if o.touches(p):
            level += 1
    for o in obs:
        c.draw(o)
    if ticks%30 ==0:
        enemies.append(gamebox.from_color(75,300,'purple',20,20))
    for e in enemies:
        if p.y > e.y:
            e.y += 2*((ticks%20)-5)
        else:
            e.y -= 2
        e.speedx += random.randrange(-5,6)
        e.move_speed()
    c.display()
    #make walls
    #make enemies
    #make key
    #make barrier to door

def level2(keys):
    c.clear('blue')
    use_obstacles()
    c.draw(p)
    for o in obs:
        c.draw(o)
    c.display()
    # in a different spot than previous levels,
    # make walls
    # make enemies
    # make key
    # make barrier to door


# def level3(keys):
    #in a different spot than previous levels,
        # make walls
        # make enemies
        # make key
        # make barrier to door

#boss level: TBD

ticks = 0
def tick(keys):
    global ticks
    global level
    global cool_down
    ticks += 1
    if level == 1:
        level1(keys)
    elif level == 2:
        level2(keys)
    if pygame.K_w in keys:
        p.speedy -= 5
    if pygame.K_s in keys:
        p.speedy += 5
    if pygame.K_d in keys:
        p.speedx += 5
    if pygame.K_a in keys:
        p.speedx -= 5
    p.speedx *= 0.5
    p.speedy *= 0.5
    if pygame.K_RIGHT in keys and cool_down <= 0:
        bullets1.append(
            gamebox.from_color(p.x, p.y, 'white', 10, 5),
        )
        cool_down = 25
        keys.remove(pygame.K_RIGHT)
    for b in bullets1:
        b.move_speed()
        bullets1[-1].speedx = 10
        bullets1[-1].center = p.center
    cool_down -= 1
    for b in bullets1:
        c.draw(b)
        for o in obs:
            if b.touches(o):
                bullets1.remove(b)
    if pygame.K_LEFT in keys and cool_down <= 0:
        bullets2.append(
            gamebox.from_color(p.x, p.y, 'white', 10, 5),
        )
        cool_down = 25
        keys.remove(pygame.K_LEFT)
    for b in bullets2:
        b.move_speed()
        bullets2[-1].speedx = -10
        bullets2[-1].center = p.center
    cool_down -= 1
    for b in bullets2:
        c.draw(b)
        for o in obs:
            if b.touches(o):
                bullets2.remove(b)
    if pygame.K_DOWN in keys and cool_down <= 0:
        bullets3.append(
            gamebox.from_color(p.x, p.y, 'white', 5, 10),
        )
        cool_down = 25
        keys.remove(pygame.K_DOWN)
    for b in bullets3:
        b.move_speed()
        bullets3[-1].speedy = 10
        bullets3[-1].center = p.center
    cool_down -= 1
    for b in bullets3:
        c.draw(b)
        for o in obs:
            if b.touches(o):
                bullets3.remove(b)
    if pygame.K_UP in keys and cool_down <= 0:
        bullets4.append(
            gamebox.from_color(p.x, p.y, 'white', 5, 10),
        )
        cool_down = 25
        keys.remove(pygame.K_UP)
    for b in bullets4:
        b.move_speed()
        bullets4[-1].speedy = -10
        bullets4[-1].center = p.center
    cool_down -= 1
    for b in bullets4:
        c.draw(b)
        for o in obs:
            if b.touches(o):
                bullets4.remove(b)
    c.draw(p)
    c.display()

gamebox.timer_loop(30, tick)
