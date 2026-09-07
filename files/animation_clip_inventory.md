# Exact Animation Clip Inventory

This document defines the exact clip set needed for the game. The goal is to avoid vague animation planning and make the production pipeline concrete enough for an animator, mocap session, or AI-assisted content pass.

## 1. Clip Design Rules

- Every core cricket action must have a clean, readable base clip.
- Critical actions should have variants for timing, power, and context.
- Clips should be authored for retargeting across the same humanoid rig.
- Support clips should not be allowed to replace cricket-specific signature motion.
- Any clip that affects gameplay perception should be validated in-game, not just in DCC software.

## 2. Player Base Locomotion

### 2.1 Idle and Ready

- `idle_relaxed`
- `idle_match_ready`
- `idle_batting_stance_front_foot`
- `idle_batting_stance_back_foot`
- `idle_bowling_start`
- `idle_fielding_close_in`
- `idle_fielding_outfield`
- `idle_wicketkeeper_crouch`

### 2.2 Movement

- `walk_forward`
- `walk_backward`
- `jog_forward`
- `jog_backward`
- `run_forward`
- `run_backward`
- `sprint_forward`
- `sprint_turn_left`
- `sprint_turn_right`
- `pivot_left`
- `pivot_right`
- `stop_short`
- `start_run`

### 2.3 General Transitions

- `turn_in_place_left`
- `turn_in_place_right`
- `step_start_left`
- `step_start_right`
- `step_stop_left`
- `step_stop_right`
- `look_to_ball`
- `react_small`
- `react_big`

## 3. Batting Clips

### 3.1 Stance and Preparation

- `bat_stance_front_foot`
- `bat_stance_back_foot`
- `bat_prepare_defensive`
- `bat_prepare_drive`
- `bat_prepare_aggressive`
- `bat_prepare_unorthodox`

### 3.2 Defensive Shots

- `bat_defensive_good`
- `bat_defensive_early`
- `bat_defensive_late`
- `bat_block_short`
- `bat_block_long`

### 3.3 Drive Shots

- `bat_drive_straight_good`
- `bat_drive_straight_early`
- `bat_drive_straight_late`
- `bat_drive_cover_good`
- `bat_drive_cover_early`
- `bat_drive_cover_late`
- `bat_drive_on_good`
- `bat_drive_on_early`
- `bat_drive_on_late`

### 3.4 Cut and Square Shots

- `bat_cut_good`
- `bat_cut_early`
- `bat_cut_late`
- `bat_square_drive_good`
- `bat_square_drive_early`
- `bat_square_drive_late`

### 3.5 Pull and Hook

- `bat_pull_good`
- `bat_pull_early`
- `bat_pull_late`
- `bat_hook_good`
- `bat_hook_early`
- `bat_hook_late`

### 3.6 Sweep and Reverse Sweep

- `bat_sweep_good`
- `bat_sweep_early`
- `bat_sweep_late`
- `bat_reverse_sweep_good`
- `bat_reverse_sweep_early`
- `bat_reverse_sweep_late`

### 3.7 Lofted and Power Shots

- `bat_loft_good`
- `bat_loft_early`
- `bat_loft_late`
- `bat_six_hit_good`
- `bat_six_hit_early`
- `bat_six_hit_late`

### 3.8 Mistimed Outcomes

- `bat_edge_outside`
- `bat_edge_inside`
- `bat_top_edge`
- `bat_miss_swing`
- `bat_miss_leave`
- `bat_inside_edge`
- `bat_late_flick_mishit`
- `bat_early_flick_mishit`

### 3.9 Running and Between Wickets

- `bat_run_start`
- `bat_run_accelerate`
- `bat_run_decelerate`
- `bat_turn_at_crease`
- `bat_dive_for_crease`
- `bat_slide_crease`
- `bat_call_yes`
- `bat_call_no`
- `bat_call_wait`

### 3.10 Batting Reactions

- `bat_react_boundary`
- `bat_react_miss`
- `bat_react_close_call`
- `bat_react_wicket`
- `bat_react_satisfied`
- `bat_react_frustrated`

## 4. Bowling Clips

### 4.1 Run-Up

- `bowl_runup_pace_short`
- `bowl_runup_pace_long`
- `bowl_runup_spin_short`
- `bowl_runup_spin_long`
- `bowl_runup_start`
- `bowl_runup_stop`

### 4.2 Pace Deliveries

- `bowl_pace_standard`
- `bowl_pace_yorker`
- `bowl_pace_bouncer`
- `bowl_pace_slower_ball`
- `bowl_pace_cutter`
- `bowl_pace_release_early`
- `bowl_pace_release_good`
- `bowl_pace_release_late`

### 4.3 Spin Deliveries

- `bowl_offspin_standard`
- `bowl_offspin_arm_ball`
- `bowl_offspin_flighted`
- `bowl_legspin_standard`
- `bowl_legspin_googley`
- `bowl_legspin_flipper`
- `bowl_spin_release_early`
- `bowl_spin_release_good`
- `bowl_spin_release_late`

### 4.4 Follow-Through

- `bowl_followthrough_pace`
- `bowl_followthrough_spin`
- `bowl_followthrough_variation`
- `bowl_recover_stance`
- `bowl_react_better_delivery`
- `bowl_react_missed_line`

### 4.5 Bowling Reactions

- `bowl_appeal_dismissal`
- `bowl_appeal_half`
- `bowl_frustrated`
- `bowl_celebrate_wicket`

## 5. Fielding Clips

### 5.1 Idle and Ready

- `field_idle_close_in`
- `field_idle_infield`
- `field_idle_outfield`
- `field_ready_crouch`
- `field_ready_upfield`

### 5.2 Ground Fielding

- `field_pickup_clean`
- `field_pickup_low`
- `field_pickup_diving`
- `field_slide_stop_left`
- `field_slide_stop_right`
- `field_block_ball`
- `field_boundary_stop`

### 5.3 Throwing

- `field_throw_short`
- `field_throw_long`
- `field_throw_underarm`
- `field_throw_flat`
- `field_throw_relay`
- `field_catch_and_throw`

### 5.4 Catching

- `field_catch_chest`
- `field_catch_low`
- `field_catch_high`
- `field_catch_diving_forward`
- `field_catch_diving_side`
- `field_boundary_catch`
- `field_dropped_catch`

### 5.5 Diving and Chase

- `field_dive_left`
- `field_dive_right`
- `field_sprint_left`
- `field_sprint_right`
- `field_lunge_save`
- `field_cutoff_run`

### 5.6 Fielding Reactions

- `field_react_out_of_reach`
- `field_react_good_stop`
- `field_react_miss`
- `field_react_direct_hit`
- `field_react_wicket_saved`

## 6. Wicketkeeper Clips

- `keeper_idle_crouch`
- `keeper_idle_ready`
- `keeper_stand_up_spin`
- `keeper_gather_ball`
- `keeper_take_edge`
- `keeper_take_low`
- `keeper_take_high`
- `keeper_stumping_start`
- `keeper_stumping_complete`
- `keeper_appeal`
- `keeper_react_runout`

## 7. Umpire Clips

- `umpire_signal_out`
- `umpire_signal_not_out`
- `umpire_signal_wide`
- `umpire_signal_no_ball`
- `umpire_signal_four`
- `umpire_signal_six`
- `umpire_signal_boundary`
- `umpire_signal_helmet`

## 8. Presentation and Meta Clips

- `intro_team_banner`
- `intro_match_pan`
- `intro_pitch_wide`
- `intro_toss_call`
- `results_win`
- `results_loss`
- `results_close_match`
- `results_stat_pop`
- `reward_unlock`
- `reward_level_up`
- `reward_new_camera`
- `reward_new_cosmetic`
- `pause_idle_loop`
- `menu_background_ambient`
- `menu_profile_focus`
- `menu_card_hover`

## 9. Replay and Cinematic Clips

- `replay_ball_trail_cut`
- `replay_batter_swing_close`
- `replay_bowler_release_close`
- `replay_wicket_close`
- `replay_boundary_close`
- `replay_fielding_dive_close`
- `replay_crowd_hit`
- `replay_long_pan`

## 10. Required Variants by Gameplay Need

### Timing Variants

Batting clips should support:

- early
- good
- late

### Power Variants

High-power actions should support:

- light
- medium
- full power

### Context Variants

Needed across clips:

- front-foot
- back-foot
- close-in fielding
- outfield fielding
- daytime
- night
- dry pitch
- green pitch

## 11. Clip Priorities

### Priority 1

- batting stance and shot matrix
- bowling run-up and delivery matrix
- fielding pickup and throw matrix

### Priority 2

- wicketkeeper set
- catcher reactions
- run-up and turn animations
- bowling follow-throughs

### Priority 3

- celebrations
- umpire signals
- replay clips
- menu and results motion

## 12. Implementation Notes

- Use shared naming conventions for all clips.
- Keep animation events aligned to gameplay frames.
- Make sure every gameplay-important clip can be blended into and out of cleanly.
- Do not rely on one generic “bat swing” clip for all shots.
- Do not rely on one generic “fielding pickup” clip for all fielding cases.

## 13. Production Checklist

- [ ] Base rig retargets correctly.
- [ ] Batting timing variants look distinct.
- [ ] Bowling actions feel cricket-specific.
- [ ] Fielding handoffs are readable.
- [ ] Keeper actions support stumping and edges.
- [ ] Presentation clips can drive intro and replay flow.
- [ ] Clips are named consistently for AI-assisted workflows.
- [ ] Every important gameplay moment has a matching animation.
