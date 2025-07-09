# GameMaker

Sommario:

- [Funzioni](#funzioni)
  - [approach()](#approach)
  - [wave()](#wave)
  - [lerp_offset()](#lerp_offset)
  - [is_mouse_hover()](#is_mouse_hover)
  - [unstuck_player()](#unstuck_player)
- [Pattern](#pattern)
  - [Cambiare dolcemente un valore in base ad un booleano](#cambiare-dolcemente-un-valore-in-base-ad-un-booleano)
  - [Screenshake](#screenshake)
  - [Particelle in sospensione](#particelle-in-sospensione)
  - [Ombra in un top down](#ombra-in-un-top-down)
  - [Pausa con screenshot](#pausa-con-screenshot)
  - [Dash](#dash)
  - [Mantenere l'ultima image di uno sprite se è finita l'animazione](#mantenere-lultima-image-di-uno-sprite-se-è-finita-lanimazione)

## Funzioni

### approach()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per raggiungere un valore pian piano, una sorta di "lerp".

```JavaScript
/// approach(_a, _b, _amount)
// Moves "a" towards "b" by "amount" and returns the result
// Nice bcause it will not overshoot "b", and works in both directions
// Examples:
//      speed = approach(speed, max_speed, acceleration);
//      hp = approach(hp, 0, damage_amount);
//      hp = approach(hp, max_hp, heal_amount);
//      x = approach(x, target_x, move_speed);
//      y = approach(y, target_y, move_speed);

function approach(_a, _b, _amount) {
    if (_a < _b) {
        _a += _amount;
        if (_a > _b)
            return _b;
    } else {
        _a -= _amount;
        if (_a < _b)
            return _b;
    }
    return _a;
}
```

### wave()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per cambiare un valore in maniera sinusoidale.

```JavaScript
//wave(from, to, duration, offset)
// Returns a value that will wave back and forth between [from-to] over [duration] seconds
// Examples
//      image_angle = wave(-45,45,1,0)  -> rock back and forth 90 degrees in a second
//      x = wave(-10,10,0.25,0)         -> move left and right quickly
 
// Or here is a fun one! Make an object be all squishy!! ^u^
//      image_xscale = wave(0.5, 2.0, 1.0, 0.0)
//      image_yscale = wave(2.0, 0.5, 1.0, 0.0)
 
function wave(from, to, duration, offset) {
    var _a4 = (to - from) * 0.5;
    return from + a4 + sin((((current_time * 0.001) + duration * offset) / duration) * (pi*2)) * _a4;
}
```

### lerp_offset()

Funzione di lerp tra due valori nel quale è possibile aggiungere un offset per rendere più facile e veloce raggiungere il valore finale

```JavaScript
//lerp_offset(_from,_to,_amount,_offset)
// Uses lerp between two values, but *_offset* is added to the difference, making it easier to reach final value
function lerp_offset(_from,_to,_amount,_offset = 1) {
    var _diff =_to - _from;
    var _diff_sign = sign(_diff);
    if _diff_sign == 0
        return _to;
       
    _diff *= _diff_sign;
    return _from + _diff_sign * min((_diff + _offset) * _amount, _diff);
}
```

### is_mouse_hover()

Verifico se il mouse è in hover sull'istanza

```JavaScript
function is_mouse_hover() {
    var _mouse_x = mouse_x;
    var _mouse_y = mouse_y;

    if (point_in_rectangle(_mouse_x, _mouse_y, bbox_left, bbox_top, bbox_right, bbox_bottom)) {
        return true
    } else {
        return false;
    }
}
```

### unstuck_player()

Script sviluppato dal buon Willshire per evitare i problemi di compenetrazione tra oggetti

```JavaScript
function unstuck_player() {
    item = obj_solid_parent; // oggetto parent dei solidi
    if instance_place(x,y,item) {
        var _ii = instance_place(x,y,item)
        if x > _ii.bbox_right {
            if (bbox_left <= _ii.bbox_right) {
                // Lascio commentato, ma è utile da tenere nel caso si voglia mostrare a schermo in quali elementi ci si incastra
                // _ii.image_blend = c_red
                x += 1;
            }
        }
        if x < _ii.bbox_left {
            if (bbox_right >= _ii.bbox_left) {
                // _ii.image_blend = c_red;
                x -= 1;
            }
        }
        if bbox_top < _ii.bbox_top {
            if (bbox_bottom >= _ii.bbox_top) {
                // _ii.image_blend = c_red
                y -= 1;
            }
        }
        if bbox_bottom > _ii.bbox_bottom {
            if (bbox_top <= _ii.bbox_bottom) {
                // _ii.image_blend = c_red
                y += 1;
            }
        }
    }
```

## Pattern

### Cambiare dolcemente un valore in base ad un booleano

```JavaScript
/// CREATE EVENT
grow = 0;
selected = false;

/// STEP EVENT
grow = lerp(grow, selected, .1);

/// DRAW (EXAMPLE)
draw_sprite_ext(sprite_index, image_index, x, y, 1+grow, 1+grow, 0+grow*45, c_white, 1)
```

### Screenshake

- Creare un Effect Layer chiamato "Screenshake" con Effect Type "Screenshake". Settare la *magnitude* a 0;
- Creare un oggetto *obj_shreenshake*

```JavaScript
/// CREATE EVENT
spd = .1;
mag = 1;

fxl = layer_get_fx("Screenshake");

/// STEP EVENT
if ( layer_exists("Screenshake") ) {
    mag = lerp(mag, 0, spd);
    fx_set_parameter(fxl, "g_Magnitude", mag);
    if ( mag <= 0.05 ) instance_destroy();
}
```

A questo punto è possibile richiamare l'effetto con ad esempio uno script del genere

```JavaScript
function screen_shake(magnitude) {
    var ins = instance_create_depth(0,0,depth,obj_screenshake);
    ins.mag = magnitude;
}
```

### Particelle in sospensione

```JavaScript
/// CREATE EVENT
psys = part_system_create();
p_type = part_type_create();

part_type_direction(p_type, 0, 360, .5, 10);
part_type_speed(p_type, .01, .2, 0, 0);
part_type_alpha3(p_type, 0, 1, 0);
part_type_color3(p_type, c_lime, c_yellow, c_red);
part_type_life(p_type, 240, 600);

repeat(10) {
    part_particles_create(psys, random(room_width), random(room_height), p_type, 1);
}

/// STEP EVENT
if ( irandom_range(-3, 1) ) { // Ho 4 probabilità su 5 che venga create una particella
    part_particles_create(psys, random(room_width), random(room_height), p_type, 1);
}

// CLEAN UP EVENT
part_type_destroy(p_type);
part_system_destroy(psys);
```

### Ombra in un top down

Esempio preso da Forgodden.
Nota: è utile creare un oggetto *obj_render* che si occupi di tutte le ombre degli oggetti

```JavaScript
with (obj_player) { // Esempio nel caso in cui si voglia crreare l'oggetto di supporto di cui sopra
    draw_sprite_ext(sprite_index, image_index, x, y, .5, 1, -90, c_black, .15);
}
```

### Pausa con screenshot

Creare un *obj_pause* che si occuperà di tutto

```JavaScript
/// CREATE EVENT
pause = false;
spr_view = -1;

/// STEP EVENT
if (keyboard_check_released(vk_escape)) {
    pause = !pause;

    if (pause) {
        spr_view = sprite_create_from_surface(application_surface, 0, 0, view_wport[0], view_hport[0], false, false, 0, 0)
        instance_deactivate_all(true)
    } else {
        instance_activate_all();
    }
}

/// DRAW GUI EVENT
if (pause) {
    draw_sprite(spr_view, 0, 0, 0);
    draw_set_halign(fa_center);
    draw_set_valign(fa_middle);
    draw_text(view_wport / 2, view_hport / 2, "--PAUSE--");
}
```

### Dash

Viene tutto gestito all'interno di obj_player, tranne per la scia che sarà dentro ad un obj_dash_trail. La gestione è fatta tramite macchina a stati

In *obj_player*

```JavaScript
/// CREATE EVENT
can_dash = true;
dash_distance = 30;
dash_time = 10;
dash_direction = 0;
dash_speed = 0;
dash_energy = 0;
dash_color = c_olive;

/// STEP EVENT
if (state == PLAYER_STATES.DASH) {
    hsp = lengthdir_x(dash_speed, dash_direction);

    // Draw Trail
    with (instance_create_depth(x, y, depth+1, obj_dash_trail)) {
        sprite_index = other.sprite_index;
        image_blend = other.dash_color;
        image_alpha = .7;
    }

    vsp = lengthdir_y(dash_speed, dash_direction);

    // Ending the dash
    dash_energy -= dash_speed;

    if (dash_energy <= 0) {
    state = PLAYER_STATES.NORMAL
    }
}

if (_key_dash && can_dash) {
    can_dash = false;
    can_jump = 0;
                    
    // Avoid dashing right if I don't press any arrow key
    if (_key_left || _key_right || _key_down || _key_up) {
        dash_direction = point_direction(0, 0,  _key_right-_key_left, _key_down - _key_up)
    } else {
        dash_direction = (dir * 90) - 90;
    }
                    
    var _ds = dash_distance / dash_time;
    dash_speed = _ds;
    dash_energy = dash_distance;
    state = PLAYER_STATES.DASH;
}
```

Ricorda di settare **can_dash = true** dove metti il controllo se il personaggio è sul terreno.

In *obj_dash_trail*

```JavaScript
/// STEP EVENT
image_alpha-=.1;

if (image_alpha <= 0) instance_destroy();
```

### Mantenere l'ultima image di uno sprite se è finita l'animazione

```JavaScript
/// STEP EVENT
if (image_index >= image_number - 1) {
    image_speed = 0;
    image_index = image_number - 1;
}
```

### Sprite Stacking semplice

```JavaScript
/// DRAW EVENT
for (var _i = 0; _i < image_number ; _i++) {
    draw_sprite_ext(sprite_index, _i, x, y - _i, image_xscale, image_yscale, image_angle, image_blend, image_alpha)
}
```

### Dare un'effetto pixelato al nostro gioco

Preso dal tutorial sullo sprite stacking di Matharoo.
Data una *camera* di dimensione 384x216, che poi viene presumibilmente stretchata cambiando la viewport di un suo multiplo, scrivere questo nel CREATE event di chi gestisce la *camera*

```JavaScript
/// CREATE EVENT
cam_w = 384;
cam_h = 216;

surface_resize(application_surface, cam_w, cam_h);
```
