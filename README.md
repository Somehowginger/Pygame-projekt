Pygame-projekt
Julian, Emil og Muzdalifa 2.P

Python (import pygame)
Visuel Studio Code

Spil antal tegn = 7319
Read me antal tegn = 7.061

Vi har lavet et simpelt run and jump spil. Man spiller som dreng som skal hoppe over snegle og undvige fluer i luften. Hvis man bliver ramt af sneglen eller fluen slutter og spillet og man kan se sin score og spille igen.
Vi har 2 player inputs i vors spil, space/mellemrum og click på musen. Space har to handlinger i vores spil. Når man spiller selve spillet, bliver space brugt til at hoppe. Hvis man er på startscreen/game over screen, så bruger man space til at starte spillet igen. Musen får kun spilleren til at hoppe.
Der er en score i spillet. Den bliver både vist når man spiller spilet men også når man er på game over screen.


Vi har rect alle vores sprites og billeder. Det er nemmere at placere rect end det øverste venstre hjørne som det normalt er. Det gør det også nemmere at lave kollision. 
def collisions(player,obstacles):
	if obstacles:
		for obstacle_rect in obstacles:
			if player.colliderect(obstacle_rect): return False
	return True
Koden tjekker om firekanterne er inde i hinanden. Ved at vi returner true eller false kan spillet slutte hvis vi bliver ramt fordi det bliver false.

 

Vi har også importet exit fra sys. Det giver os kommandoen. 
if event.type == pygame.QUIT: 
  pygame.quit()   
  exit()
Det gør det mere simpelt at lukke spillet


Choice fra random har vi også importet. Det giver os mulighed for vores obstacles at komme tilfældigt nemt. 
if game_active:
			if event.type == obstacle_timer:
				obstacle_group.add(Obstacle(choice(['fly','snail','snail','snail'])))
Her bestemmer programmet selv hvilken obstacle der skal komme som den næste. Vi har valgt, at der kun skal være 25 procent chance, for en flue siden man ikke skal reagere på den.


Vi ville gerne lave en startsscreen/game over screen, så vi lavede variablen game_active
Når man starter spillet er game_active=False. Det vil sige vi starter på startscreen. Hvis man så klikker space bliver game_active=True. Så starter spillet.

if score == 0: screen.blit(game_message,game_message_rect)
else: screen.blit(score_message,score_message_rect)
De 2 linjer kode gør hvis man har en score på 0 står der "Press space to run". Siden man starter spillet med en score på 0 står der altid "Press space to run" første gang. 
Scoren når også at blive 1 før man skal dø til den, hvilket gør så man kun får "Press space to run" en gang, og det er den første gang.
Ellers står der ens score.


Vi har fem forskellige versioner af vores spil. 
I den første version har vi vores background, ground, vores spiller med bevægelse og animation.
I den anden version lavede vores obstacles, deres bevægelse og deres animation
I den tredje version lavede vi collision mellem vores player og obstacles. Så man kan tabe i spillet. Før gik de bare igennem hinanden og der skete ikke noget.
I den fjerde version lavede vi et score system så man kan se sin score. Så kan man også holde styr på hvor langt man er kommet og hvor god man er.
I den femte, som er den sidste, version tilføj vi musik i baggrunden, og vi lavede en starts og game over screen.

Vores player og obstacles har to billeder til at skabe deres animation. Vi har brugt et program der hedder aseprite til at lave dem. Aseprite er et godt program hvis man vil lave pixelart. 
Teksttypen der bliver brugt ligner også lidt pixelart for at matche spillet stil.

Vors animation er simpel. Med spilleren tjekker vi om man er i luften eller på jorden. Hvis man er i luften får man hoppe billedet, og hvis man er på jorden skifter den mellem to billeder.
def animation_state(self):
		if self.rect.bottom < 300: 
			self.image = self.player_jump
		else:
			self.player_index += 0.1
			if self.player_index >= len(self.player_walk):self.player_index = 0
			self.image = self.player_walk[int(self.player_index)]

Vi har også lavet en class for vores player og obstacles:
class Player(pygame.sprite.Sprite):
	def __init__(self):
		super().__init__()
		player_1 = pygame.image.load('graphics/player_1.png').convert_alpha()
		player_2 = pygame.image.load('graphics/player_2.png').convert_alpha()
		self.player_walk = [player_1,player_2]
		self.player_index = 0
		self.player_jump = pygame.image.load('graphics/player_1.png').convert_alpha()
  		self.image = self.player_walk[self.player_index]
		self.rect = self.image.get_rect(midbottom = (80,300))
		self.gravity = 0
		self.jump_sound = pygame.mixer.Sound('audio/jump.mp3')
		self.jump_sound.set_volume(0.5)
	def player_input(self):
		keys = pygame.key.get_pressed()
		if keys[pygame.K_SPACE] and self.rect.bottom >= 300:
			self.gravity = -20
			self.jump_sound.play()
	def apply_gravity(self):
		self.gravity += 1
		self.rect.y += self.gravity
		if self.rect.bottom >= 300:
			self.rect.bottom = 300
	def animation_state(self):
		if self.rect.bottom < 300: 
			self.image = self.player_jump
		else:
			self.player_index += 0.1
			if self.player_index >= len(self.player_walk):self.player_index = 0
			self.image = self.player_walk[int(self.player_index)]
	def update(self):
		self.player_input()
		self.apply_gravity()
		self.animation_state()

class Obstacle(pygame.sprite.Sprite):
	def __init__(self,type):
		super().__init__()
		if type == 'fly':
			fly_1 = pygame.image.load('graphics/fly1.png').convert_alpha()
			fly_2 = pygame.image.load('graphics/fly2.png').convert_alpha()
			self.frames = [fly_1,fly_2]
			y_pos = 210
		else:
			snail_1 = pygame.image.load('graphics/snail1.png').convert_alpha()
			snail_2 = pygame.image.load('graphics/snail2.png').convert_alpha()
			self.frames = [snail_1,snail_2]
			y_pos  = 300
		self.animation_index = 0
		self.image = self.frames[self.animation_index]
		self.rect = self.image.get_rect(midbottom = (randint(900,1100),y_pos))
	def animation_state(self):
		self.animation_index += 0.1 
		if self.animation_index >= len(self.frames): self.animation_index = 0
		self.image = self.frames[int(self.animation_index)]
	def update(self):
		self.animation_state()
		self.rect.x -= 6
		self.destroy()
	def destroy(self):
		if self.rect.x <= -100: 
			self.kill()
player = pygame.sprite.GroupSingle()
player.add(Player())

Vi synes vores kode var meget rodet, fordi vi har mange forskellige dele til vores player og obstacles. Så vi lavede en class for at samle de dele. Det giver nogle ekstra self commands for at holde styr på alle de variabler vi har lavet. 
Class gøre så billede og rect billede bliver den samme. Så skal vi skrive halvt så meget kode for at flytte vores player og obstacle og deres kollision.







![image](https://github.com/Somehowginger/Pygame-projekt/assets/74010507/984037e1-f392-4af9-80a2-5ed57c041295)
På billedet kan an se vores flowdiagram. Flowdiagrammt er meget simpelt, ligesom spillet. Siden der ikke sker så meget. Det sekund spillet starter, begynder fjenderne at spawne i højre del af skærmen, i tilfældige højder. Hvis karakteren rør en karakter så stopper alt og skærmen skifter til slutskærmen. Man undviger fjenderne ved at hoppe. Dette gør man ved at trykke på mellmrum. Karakteren skifter derefter til hoppe animationen, og karakteren flyver op og bagefter ned igen, og på den måde simulerer et hop.


