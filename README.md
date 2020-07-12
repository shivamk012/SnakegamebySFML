# SnakegamebySFML
A simple snake game created by in C++ using sfml library
#include<SFML/Graphics.hpp>
#include<vector>
#include<time.h>


class Game {
private:
	sf::RenderWindow window;
	sf::Texture t1, t2, t3;	
	sf::Sprite cell, food;
	std::vector<sf::Sprite> snake;
	sf::Vector2f dir , v1 ,v2 , food_location; 

private:
	void processevents();
	void update();
	void render();
	void append_snake();
public:

	Game() {
		window.create(sf::VideoMode(496, 496) , "Snake" , sf::Style::Close);
		window.setFramerateLimit(15);
		
			t1.loadFromFile("C:/Users/Hp/Desktop/My Folder/red.png");
			t2.loadFromFile("C:/Users/Hp/Desktop/My Folder/green.png");
			t3.loadFromFile("C:/Users/Hp/Desktop/My Folder/white.png");
			cell.setTexture(t3);food.setTexture(t2);
			dir.x = 1, dir.y = 0;

			sf::Sprite s(t1);
			s.setPosition(sf::Vector2f(0, 0));
			snake.push_back(s);
	}

	void game_loop() {
		while (window.isOpen()) {
			processevents();
			update();
			render();
		}
	}

};

void Game::processevents() {
	sf::Event evnt;
	if (window.pollEvent(evnt)) {
		if (evnt.type == evnt.Closed)
			window.close();
		else if (evnt.type == sf::Event::KeyPressed) {
			if (evnt.key.code == sf::Keyboard::Right && dir.x!=-1)
			{
				dir.x = 1; dir.y = 0;
			}
			else if (evnt.key.code == sf::Keyboard::Left && dir.x!=1)
			{
				dir.x = -1; dir.y = 0;
			}
			else if (evnt.key.code == sf::Keyboard::Up && dir.y!=1)
			{
				dir.y = -1;dir.x = 0;
			}
			else if (evnt.key.code == sf::Keyboard::Down && dir.y!=-1)
			{
				dir.y = 1;dir.x = 0;
			}
		}
	}
}

void Game::update() {
	v1.x = snake[0].getPosition().x;
	v1.y = snake[0].getPosition().y;
	snake[0].move(sf::Vector2f(dir.x*16, dir.y*16));
	window.draw(snake[0]);
	for (int j = 1;j < snake.size();++j) {
		if (j % 2 != 0) {
			v2.x = snake[j].getPosition().x;
			v2.y = snake[j].getPosition().y;
			snake[j].setPosition(v1);
		}
		else {
			v1.x = snake[j].getPosition().x;
			v1.y = snake[j].getPosition().y;
			snake[j].setPosition(v2);
		}
	}
	if (v1.x == food_location.x && v1.y == food_location.y) {
		food_location.x = (rand() % 31) * 16;
		food_location.y = (rand() % 31) * 16;
		food.setPosition(food_location);
		append_snake();
	}
	else if (snake[0].getPosition().x > 496 || snake[0].getPosition().x < 0)
		window.close();
	else if (snake[0].getPosition().y < 0 || snake[0].getPosition().y> 496)
		window.close();
	else {
		for (size_t j = 2;j < snake.size();++j) {
			if (snake[0].getPosition().x== snake[j].getPosition().x && snake[0].getPosition().y== snake[j].getPosition().y) {
				window.close();
			}
		}
	}
}

void Game::render() {
	window.clear(sf::Color::White);
	for (int i = 0; i < 496 / 16; ++i) {
		for (int j = 0; j < 496 / 16; ++j) {
			cell.setPosition(sf::Vector2f(16 * j, 16 * i));
			window.draw(cell);
		}
	}
	
	window.draw(food);

	for (size_t i = 0; i < snake.size(); ++i) {
		window.draw(snake[i]);
	}

	window.display();
}

void Game::append_snake() {	
	sf::Sprite s(t1);
	sf::Vector2f p(snake[snake.size() - 1].getPosition().x , snake[snake.size() - 1].getPosition().y);
	s.setPosition(sf::Vector2f(p.x + 16 * dir.x, p.y + 16 * dir.y));
	snake.push_back(s);
}

int main() {
	srand(time(0));
	Game game;
	game.game_loop();

}
