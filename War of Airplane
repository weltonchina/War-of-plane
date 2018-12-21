import pygame
import random
from os import path

WIDTH, HEIGHT = 480,600
NEW_EMENY_GENERATE_INTERVAL = 500
BLACK = (0,0,0)
WHITE = (255,255,255)

MISSILE_LIFETIME = 10000
MISSILE_INTERCVAL = 500

class Player(pygame.sprite.Sprite):
	
	def __init__(self):
		pygame.sprite.Sprite.__init__(self)
		self.image = pygame.transform.flip(player_img,False,True)
		self.image = pygame.transform.scale(self.image,(53,40))
		self.image.set_colorkey((0,0,0))
		
		self.rect = self.image.get_rect()
		self.rect.centerx = WIDTH/2
		self.rect.bottom = HEIGHT
		self.hp = 100
		self.lives = 3
		self.score = 0
		self.hidden = False
		self.hide_time = 0
		self.is_missile_firing = False
		self.start_misslie_time = 0
		self.last_missile_time = 0

	def update(self):
		key_state = pygame.key.get_pressed()
		if key_state[pygame.K_LEFT]:
			self.rect.x -=8
		if key_state[pygame.K_RIGHT]:
			self.rect.x +=8
		if self.rect.right>WIDTH:
		    self.rect.right = WIDTH
		if self.rect.left<0:
		 	self.rect.left = 0

		now = pygame.time.get_ticks()
		if self.hidden and now - self.hide_time >1000:
		    self.hidden = False
		    self.rect.bottom = HEIGHT

		if self.is_missile_firing:
			if now - self.start_misslie_time < MISSILE_LIFETIME:
				if now - self.last_missile_time > 500 :
					missile = Missile(self.rect.x,self.rect.y)
					missiles.add(missile)
					self.last_missile_time = now
			else:
		    	 self.is_missile_firing = False	


		    	 	   	   
		 		


	def shoot(self):
		bullet = Bullet(self.rect.centerx,self.rect.centery)
		bullets.add(bullet)

	def hide(self):
			self.hidden = True
			self.rect.y = -200
			self.hide_time = pygame.time.get_ticks()

	def fire_missile(self):
		self.is_missile_firing = True
		self.start_misslie_time = pygame.time.get_ticks()
								
class Enemy(pygame.sprite.Sprite):
	
	def __init__(self):
		pygame.sprite.Sprite.__init__(self)
		img_width = random.randint(20,120)
		self.image = pygame.transform.scale(emeny_img,(img_width,img_width))
		self.image.set_colorkey((0,0,0))
		self.origin_image = self.image.copy()
		self.rect = self.image.get_rect()
		self.radius = img_width/2
		self.rect.x = random.randint(0,WIDTH-self.rect.w)
		self.rect.bottom = 0
		self.time = pygame.time.get_ticks()
		self.rotate_speed = random.randint(-5,5)
		self.rotate_angel = 0

		self.vx = random.randint(-2,2)
		self.vy = random.randint(2,5)
	def update(self):
		self.rect.x += self.vx	
		self.rect.y += self.vy
		self.rotate()

	def rotate(self):
			now = pygame.time.get_ticks()
			if now - self.time > 30:
				self.rotate_angel = (self.rotate_angel + self.rotate_speed)%360
				old_center = self.rect.center

				self.image = pygame.transform.rotate(self.origin_image, self.rotate_angel)
				self.rect = self.image.get_rect()
				self.rect.center = old_center
class Bullet(pygame.sprite.Sprite):
	
	def __init__(self, x, y):
		pygame.sprite.Sprite.__init__(self)
		self.image = bullet_img
		self.image.set_colorkey((0,0,0))
		self.rect = self.image.get_rect()
		self.rect.centerx = x
		self.rect.centery = y


	def  update(self):
			self.rect.y -= 10	

class Missile(pygame.sprite.Sprite):
	
	def __init__(self, x, y):
		pygame.sprite.Sprite.__init__(self)
		self.image = missile_img
		self.image.set_colorkey((0,0,0))
		self.rect = self.image.get_rect()
		self.rect.centerx = x
		self.rect.centery = y


	def  update(self):
			self.rect.y -= 5	





class Explosion(pygame.sprite.Sprite):
	
	def __init__(self, center):
		pygame.sprite.Sprite.__init__(self)
		self.image = pygame.transform.scale(explosion_animation[0],(80,80))
		self.image = explosion_animation[0]
		self.image.set_colorkey((0,0,0))
		self.rect = self.image.get_rect()
		self.rect.center = center
		self.frame = 0
		self.last_time = pygame.time.get_ticks()



	def update(self):
		now = pygame.time.get_ticks()
		if now - self.last_time > 90:

			if  self.frame < len(explosion_animation) :
				 self.image = explosion_animation[self.frame]
				 self.image.set_colorkey((0,0,0))
				 self.frame  += 1
				 self.last_time = pygame.time.get_ticks()

			else:
				 self.kill()

class Powerup(pygame.sprite.Sprite):
	"""docstring for Powerup"""
	def __init__(self,center):
		pygame.sprite.Sprite.__init__(self)
		random_num = random.random()
		if random_num>=0 and random_num<0.5:
			self.type = 'add_hp'
		elif random_num>=0.5 and random_num<=0.8:
			self.type = 'add_missile'
		else :
			self.type = 'add_life'
		self.image = powerup_imgs[self.type]
		self.rect = self.image.get_rect()
		self.image.set_colorkey(BLACK)
		self.rect.center = center

	def update(self):
		self.rect.y += 4
				

			

			
		





def draw_ui():
	pygame.draw.rect(screen,(0,255,0), (10,10,player.hp,15))
	pygame.draw.rect(screen,(0,255,0), (10,10,100,15),2)
	draw_text(str(player.score), screen, (255,255,255),WIDTH/2,10)
	img_rect = player_img_small.get_rect()
	img_rect.x = WIDTH - (img_rect.width + 10)
	img_rect.y = 10
	for i in range(player.lives):
		screen.blit(player_img_small,img_rect)
		img_rect.x -= img_rect.width +10


def draw_text(text,surface,color,x,y):
	font_name = pygame.font.match_font('arial')
	font = pygame.font.Font(font_name,20)
	text_surface = font.render(text,True,color)
	text_rect = text_surface.get_rect()
	text_rect.midtop = (x,y)
	surface.blit(text_surface,text_rect)


def  show_meun():
		global game_state, screen
		screen.blit(background_img, background_rect)
		draw_text('Space Shooter',screen, WHITE,  WIDTH/2, 100)	
		draw_text('Press Space key to start', screen, WHITE,  WIDTH/2, 300)
		draw_text('Press quit', screen, WHITE,  WIDTH/2, 400)

		event_list = pygame.event.get()
		for event in event_list:
			
			if event.type ==pygame.QUIT:
				pygame.quit()
				quit()


			if event.type == pygame.KEYDOWN:
				if event.key == pygame.K_ESCAPE:
					pygame.quit()
					quit()
				if event.key == pygame.K_SPACE:
	   				game_state = 1

		pygame.display.flip()			

			

game_state = 0	

			
last_enemy_generate_time = 0				 



pygame.init()
screen = pygame.display.set_mode((WIDTH,HEIGHT))
pygame.display.set_caption("My Game")
clock = pygame.time.Clock()

img_dir = path.join(path.dirname(__file__),'img')
background_dir = path.join(img_dir,'background.png')
background_img = pygame.image.load(background_dir).convert()
background_rect = background_img.get_rect()
player_dir = path.join(img_dir,'player.png')
player_img = pygame.image.load(player_dir).convert()
player_img_small = pygame.transform.scale(player_img,(26,20))
player_img_small.set_colorkey((0,0,0))
emeny_dir = path.join(img_dir,'emeny.png')
emeny_img = pygame.image.load(emeny_dir).convert()
bullet_dir = path.join(img_dir,'bullet.png')
bullet_img = pygame.image.load(bullet_dir).convert()
missile_dir = path.join(img_dir,'spaceMissiles_021.png')
missile_img = pygame.image.load(missile_dir).convert()

powerup_imgs = {}
powerup_add_up_dir = path.join(img_dir,'spaceEffects_010.png')
powerup_imgs['add_hp'] = pygame.image.load(powerup_add_up_dir).convert()
powerup_add_life_dir = path.join(img_dir,'spaceEffects_011.png')
powerup_imgs['add_life'] = pygame.image.load(powerup_add_life_dir).convert()
powerup_add_missile_dir = path.join(img_dir,'spaceEffects_012.png')
powerup_imgs['add_missile'] = pygame.image.load(powerup_add_life_dir).convert()


explosion_animation = []
for i in range(9):
	explosion_dir = path.join(img_dir,'regularExplosion0{}.png'.format(i))
	explosion_img = pygame.image.load(explosion_dir).convert()
	explosion_animation.append(explosion_img)
player = Player()


enemys = pygame.sprite.Group()
for i in range(10):
	enemy = Enemy()	
	enemys.add(enemy)

bullets = pygame.sprite.Group()	
explosions = pygame.sprite.Group()
powerups = pygame.sprite.Group()
missiles = pygame.sprite.Group()



game_over = False
while not game_over:
	clock.tick(60)
	if game_state == 0:
		show_meun()
	else:	
		
		event_list = pygame.event.get()
		for event in event_list:
			print(event)
			if event.type ==pygame.QUIT:
				game_over = True
			if event.type == pygame.KEYDOWN:
				if event.key == pygame.K_ESCAPE:
					game_over = True
				if event.key == pygame.K_SPACE:
	   				player.shoot()

			if event.type == pygame.MOUSEMOTION:
			    mouse_x, mouse_y = event.pos
			    print(mouse_x,mouse_y)		

		mouse_x, mouse_y = pygame.mouse.get_pos()

		now = pygame.time.get_ticks()
		if now - last_enemy_generate_time > NEW_EMENY_GENERATE_INTERVAL:
			enemy = Enemy()
			enemys.add(enemy)
			last_enemy_generate_time = now
			


		hits = pygame.sprite.spritecollide(player,enemys,True)
		for hit in hits:
			player.hp -= hit.radius
			if player.hp <=0:
				player.lives -= 1
				player.hp = 100
				player.hide()
				if player.lives==0:
					game_over = True

		hits = pygame.sprite.groupcollide(enemys,bullets,True,True)	
		for hit in hits:
			explosion = Explosion(hit.rect.center)	
			explosions.add(explosion)
			player.score += 80 - hit.radius
			if random.random() > 0.1:
				powerup = Powerup(hit.rect.center)
				powerups.add(powerup)

		screen.fill((255,255,255))

		hits = pygame.sprite.spritecollide(player,powerups,True)
		for hit in hits:
			if hit.type == 'add_hp' :
				player.hp +=50
				if player.hp >100:
					player.hp = 100
			elif hit.type == 'add_hp' :
				player.lives += 1
				if player.lives >3:
					player.lives = 3
			else:
			    player.fire_missile()	


		hits = pygame.sprite.groupcollide(enemys,missiles,True,True)	
		for hit in hits:
			explosion = Explosion(hit.rect.center)	
			explosions.add(explosion)
			player.score += (80 - hit.radius)*2
			if random.random() > 0.1:
				powerup = Powerup(hit.rect.center)
				powerups.add(powerup)
		    	


					



		player.update()
		enemys.update()
		bullets.update()
		explosions.update()
		powerups.update()
		missiles.update()

		screen.blit(background_img,background_rect)

		screen.blit(player.image, player.rect)
		enemys.draw(screen)
		bullets.draw(screen)
		powerups.draw(screen)
		explosions.draw(screen)
		missiles.draw(screen)

		draw_ui()


		pygame.display.flip()

