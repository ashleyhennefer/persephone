persephone
==========

Code for game
///Particals///
///create///

Sname=part_system_create()
 
 particle1 = part_type_create();
part_type_shape(particle1,pt_shape_square);
part_type_size(particle1,0.01,0.03,0,0);
part_type_scale(particle1,1,1);
part_type_color3(particle1,5614335,33023,33023);
part_type_alpha3(particle1,1,1,1);
part_type_speed(particle1,1,2,0,2);
part_type_direction(particle1,80,100,0,10);
part_type_gravity(particle1,0.05,270);
part_type_blend(particle1,0);
part_type_life(particle1,1,50);
emitter1 = part_emitter_create(Sname);


part_emitter_region(Sname,emitter1,x-view_xview-20,x-view_xview+20,y-view_yview,y-view_yview,0,0);
part_emitter_stream(Sname,emitter1,particle1,1);












///create////
/// Initialise the baddie.
xp = -1;
image_speed = 0.5;
hit = false;
grav = 0;
dirspeed = 0;

child = -1;
AddMultiple(id);

// There is also some creation code inside the ROOM which sets up the path and colour.
// To see this, right click the instance in the map and select "creation code"




///step///
/// Check the direction of the baddie. 
if( xp>x ) {
    if(  sprite_index != walk_left ) sprite_index = walk_left;
}else{
    if(  sprite_index != walk_right ) sprite_index = walk_right;
}
xp=x;


// If we've been hit, this means we're no longer 
// following a path, so bounce him off sctreen!
if( hit ){
    x = x+dirspeed;
    y = y + grav;
    grav+=0.4;
    if( grav>=10 ) grav=10;

    // Once we've fallen below the room, kill him!
    if( y>room_height ) instance_destroy();
}





///endstep///
size = 80;
//light= surface_create(view_wview,view_hview);
draw_set_blend_mode(bm_subtract);
surface_set_target(light);
draw_ellipse_color((x+25)-size/2-view_xview,y-size/2-view_yview,x+size/2-view_xview,y+size/2-view_yview,c_ltgray,c_black,false);

surface_reset_target();
draw_set_blend_mode(bm_normal);

////collide (with player)////
/// Kill baddie when player hits it!

// Make sure we're not colliding over and over again!
if( hit ) exit;

// Attach our multiple to the player.
AttachMultiple(player.id, child);
child = -1;

// Kill the path we're following.
path_end();

// Flag as hit, and set the gravity mover to UP.
hit = true;
grav = -8;

// Bounce in the correct direction.
if(other.x>x ){
    dirspeed = -4;
}else{
    dirspeed = 4;
}



////minime///////////////////////////////////////////////////////////////////////////
///create////
/// Initialise the "mini me"
xp=x;
sprite_index = walk_right;
image_speed = 0.5;
image_xscale= 0.75;
image_yscale= 0.75;
child = -1;

msize = 20;
mx = ds_queue_create();
my = ds_queue_create();
ms = ds_queue_create();
ma = ds_queue_create();

for(i=0;i<msize;i+=1){
    ds_queue_enqueue(mx,x);
    ds_queue_enqueue(my,y);
    ds_queue_enqueue(ms,sprite_index);
    ds_queue_enqueue(ma,image_index);
}

destid = id;


///step///
/// Update the position and animation of the mini-me


// get last location, and the animation frames...
x = ds_queue_dequeue(mx);
y = ds_queue_dequeue(my);
sprite_index = ds_queue_dequeue(ms);
image_index = ds_queue_dequeue(ma);

// Queue the NEXT location
ds_queue_enqueue(mx,destid.x);
ds_queue_enqueue(my,destid.y);
ds_queue_enqueue(ms,destid.sprite_index);
ds_queue_enqueue(ma,destid.image_index);

///drsw////
/// Draw the mini-me.

// Offset here so that we can easily attach to other mini-me's
draw_sprite_ext(sprite_index, image_index, x+4, y+8, image_xscale,image_yscale, 0, image_blend, 1.0);



