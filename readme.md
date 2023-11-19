[![crates.io](https://img.shields.io/crates/v/ggegui)](https://crates.io/crates/ggegui)
[![docs.rs](https://img.shields.io/docsrs/ggegui)](https://docs.rs/ggegui/)
# ggegui
An [egui](https://github.com/emilk/egui/) implementation for the [ggez](https://ggez.rs/) game framework

## Ultra minimal example
```rust
use ggegui::{egui, Gui};
use ggez::{
	ContextBuilder, Context, GameResult, glam,
	event::{ self, EventHandler}, 
	graphics::{ self, DrawParam, Color }
};

fn main() {
	let (mut ctx, event_loop) = ContextBuilder::new("game_id", "author").build().unwrap();
	let state = State::new(&mut ctx);
	event::run(ctx, event_loop, state);
}

struct State {
	gui: Gui,
} 

impl State {
	pub fn new(ctx: &mut Context) -> Self {
		Self { 
			gui: Gui::new(ctx),
		}
	} 
} 

impl EventHandler for State {
	fn update(&mut self, ctx: &mut Context) -> GameResult {
		let gui_ctx = self.gui.ctx();

		egui::Window::new("Title").show(&gui_ctx, |ui| {
			ui.label("label");
			if ui.button("button").clicked() {
				println!("button clicked");
			}
		});
		self.gui.update(ctx);
		Ok(())
	}

	fn draw(&mut self, ctx: &mut Context) -> GameResult {
		let mut canvas = graphics::Canvas::from_frame(ctx, Color::BLACK);
		canvas.draw(
			&self.gui, 
			DrawParam::default().dest(glam::Vec2::ZERO),
		);
		canvas.finish(ctx)
	}
}
```

there are a few [examples](./examples/) to show how to use this implementation.
