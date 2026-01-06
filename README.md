# SFML
A C++ application built with SFML that demonstrates basic game mechanics and physics.  The project features a movable square with acceleration and deceleration, smooth movement control, and collision detection.  When a collision occurs, a sound effect is played and the square changes its color, providing visual and audio feedback.

#include<iostream>
#include<SFML/Graphics.hpp>
using namespace std;
using namespace sf;
 
int main(){
	//Announce the window
	RenderWindow window(VideoMode(800,600),"SFML GAMING");
	//Adjusting the movement in the window SFML
	window.setFramerateLimit(60);

	//Creating a circle shabe
	CircleShape circle(50);
	circle.setFillColor(Color::Green);
	FloatRect CircleBounds = circle.getLocalBounds();
	circle.setOrigin(CircleBounds.width / 2, CircleBounds.height / 2);
	circle.setPosition(200, 200);
	circle.setOutlineThickness(6);
	circle.setOutlineColor(Color::Blue);
	//Creating a square shape
	RectangleShape square(Vector2f(80, 80));
	square.setFillColor(Color::Transparent);
	FloatRect SquareBounds = square.getLocalBounds();
	square.setOrigin(SquareBounds.width / 2, SquareBounds.height / 2);
	square.setPosition(400, 300);
	square.setOutlineThickness(6);
	square.setOutlineColor(Color::White);
	//movement speed and direction
	float speed = 5.0f;
	int driection = 1;
	Vector2f velocity(0.f, 0.f);
	float acceleration = 800.f;
	float deceleration = 600.f;
	float mexsleep = 300.f;
	//Setting up the clock
	Clock clock;
	//Main program episode
	while (window.isOpen()) {
		//Getting the deleta time
		float deltaTime = clock.restart().asSeconds();
		//Event polling 

		Event event;
		while (window.pollEvent(event)) {
			if (event.type == Event::Closed) {
				window.close();
			}

		}

		//Moving circle
		circle.move(speed * driection, 0);
		if (circle.getPosition().x + 50 > window.getSize().x) {
			driection = -1;

		}
		else if (circle.getPosition().x - 50 < 0) {
			driection = 1;
		}

		//Moving square
		bool Right = Keyboard::isKeyPressed(Keyboard::Right);
		bool Left = Keyboard::isKeyPressed(Keyboard::Left);
		bool Up = Keyboard::isKeyPressed(Keyboard::Up);
		bool Down = Keyboard::isKeyPressed(Keyboard::Down);
		if (Right && !Left) {
			velocity.x = velocity.x + acceleration * deltaTime;
		}
		else if (Left && !Right) {
			velocity.x = velocity.x - acceleration * deltaTime;
		}
		else {
			if (velocity.x > 0) {
				velocity.x = velocity.x - deceleration * deltaTime;
				if (velocity.x < 0) {
					velocity.x = 0;
				}
			}
			else if (velocity.x < 0) {
				velocity.x = velocity.x + deceleration * deltaTime;
				if (velocity.x > 0) {
					velocity.x = 0;
				}
			}
		}
		if (Up && !Down) {
			velocity.y = velocity.y - acceleration * deltaTime;
		}
		else if (Down && !Up) {
			velocity.y = velocity.y + acceleration * deltaTime;
		}
		else {
			if (velocity.y < 0) {
				velocity.y = velocity.y + deceleration * deltaTime;
				if (velocity.y > 0) {
					velocity.y = 0;
				}
			}
			else if (velocity.y > 0) {
				velocity.y = velocity.y - deceleration * deltaTime;
				if (velocity.y < 0) {
					velocity.y = 0;
				}
			}
		}

		//Limiting the maximum speed
		if (velocity.x > mexsleep) {
			velocity.x = mexsleep;
		}
		if (velocity.x < -mexsleep) {
			velocity.x = -mexsleep;
		}
		if (velocity.y > mexsleep) {
			velocity.y = mexsleep;
		}
		if (velocity.y < -mexsleep) {
			velocity.y = -mexsleep;
		}

		//Updating the position
		square.move(velocity * deltaTime);

		//After the movement, check for the collision
		if (circle.getGlobalBounds().intersects(square.getGlobalBounds())) {
			square.setFillColor(Color::Red);
		}
		else {
			square.setFillColor(Color::Transparent);
		}

		//Clearing the window
		window.clear(Color::Black);
		//Drawing the shapes
		window.draw(circle);
		window.draw(square);
		//Displaying the window
		window.display();

	}
	return 0;
}
