
# Bevy Points

[![crates.io](https://img.shields.io/crates/v/bevy_points)](https://crates.io/crates/bevy_points)

Points mesh plugin for Bevy.

![Example](https://user-images.githubusercontent.com/1085910/202979410-3bc4bf60-b4af-421c-b2ab-6ac08e00f6a6.png)

## Usage

### System setup

Add the plugin to your app:

```rust
use bevy::prelude::*;
use bevy_points::prelude::*;

fn main() {
    App::new()
        .add_plugin(PointsPlugin);
}
```

### Apply a component to a MaterialMeshBundle

```rust
fn setup(
    mut commands: Commands,
    mut meshes: ResMut<Assets<Mesh>>,
    mut materials: ResMut<Assets<PointsMaterial>>,
) {
    let n = 320; // # of points
    let h = 3.0; // spiral height
    commands.spawn(MaterialMeshBundle {
        mesh: meshes.add(
            PointsMesh::from_iter((0..n).map(|i| {
                let t01 = (i as f32) / ((n - 1) as f32);
                let r = t01 * TAU * 4.0; // spiral fineness
                Vec3::new(r.cos(), (t01 - 0.5) * h, r.sin())
            }))
            .into(),
        ),
        material: materials.add(PointsMaterial {
            settings: PointsShaderSettings {
                point_size: 20.,    // Defines the size of the points. 
                ..Default::default()
            },
            perspective: true,      // Specify whether points' size is attenuated by the camera depth. 
            circle: true,           // Specify whether the shape of points is circular or rectangular.
            ..Default::default()
        }),
        ..Default::default()
    });

    commands.spawn(MaterialMeshBundle {
        mesh: meshes.add(
            // Mesh can be converted to PointsMesh.
            PointsMesh::from(Mesh::from(shape::Icosphere::default()))
            .into()
        ),
        material: materials.add(PointsMaterial {
            settings: PointsShaderSettings {
                color: Color::BLUE,
                opacity: 0.5,
                ..Default::default()
            },
            alpha_mode: AlphaMode::Blend,
            ..Default::default()
        }),
        ..Default::default()
    });
}

```

## Compatibility

| bevy | bevy_points |
| ---- | ------------- |
| 0.9  | 0.1           |
| 0.10 | 0.2           |
| 0.11 | 0.3           |
| 0.12 | 0.4           |
| 0.13 | 0.5           |
