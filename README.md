<?php
// Register Custom Post Type for Projects
function register_projects_post_type() {
    $args = array(
        'labels'        => array(
            'name'          => 'Projects',
            'singular_name' => 'Project',
        ),
        'public'        => true,
        'has_archive'   => true,
        'menu_icon'     => 'dashicons-portfolio',
        'supports'      => array('title', 'editor', 'thumbnail'),
    );
    register_post_type('projects', $args);
}

add_action('init', 'register_projects_post_type');

// Register Taxonomies for Skills
function register_skills_taxonomy() {
    $args = array(
        'labels'            => array(
            'name'          => 'Skills',
            'singular_name' => 'Skill',
        ),
        'public'            => true,
        'hierarchical'      => true,
    );
    register_taxonomy('skills', 'projects', $args);
}

add_action('init', 'register_skills_taxonomy');

// Shortcode to Display Projects and Skills
function display_projects_skills() {
    $output = '<div class="projects">';
    $projects = new WP_Query(array('post_type' => 'projects'));

    if ($projects->have_posts()) : 
        while ($projects->have_posts()) : $projects->the_post();
            $output .= '<div class="project">';
            $output .= '<h3>' . get_the_title() . '</h3>';
            $output .= '<div>' . get_the_content() . '</div>';
            $output .= '</div>';
        endwhile;
    endif;

    wp_reset_postdata();
    $output .= '</div>';
    return $output;
}

add_shortcode('display_projects', 'display_projects_skills');

?>
