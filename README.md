persephone
==========

Code for game



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



