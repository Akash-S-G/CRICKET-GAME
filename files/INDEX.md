# Docs Index

This index links the main design and production documents in this repo.

## Core Design

- [GDD.md](GDD.md)
- [system_design.md](system_design.md)
- [system_design/INDEX.md](system_design/INDEX.md)
- [implementation_master_plan.md](implementation_master_plan.md)
- [milestone_checklist.md](milestone_checklist.md)
- [feature_implementation_master.md](feature_implementation_master.md)
- [TECH_STACK.md](TECH_STACK.md)
- [development_plan.md](development_plan.md)
- [roadmap.md](roadmap.md)
- [mobile_roadmap.md](mobile_roadmap.md)

## Gameplay Flow

- [game_flow_and_camera_design.md](game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](player_onboarding_and_tutorial_flow.md)
- [controller_handoff_spec.md](controller_handoff_spec.md)
- [rules_engine_spec.md](rules_engine_spec.md)
- [animation_requirements.md](animation_requirements.md)

## Animation and Scenes

- [animation_and_scene_pipeline_roadmap.md](animation_and_scene_pipeline_roadmap.md)
- [animation_clip_inventory.md](animation_clip_inventory.md)
- [scene_by_scene_setup.md](scene_by_scene_setup.md)

## Feature Docs

- [features/00_foundation_and_tooling.md](features/00_foundation_and_tooling.md)
- [features/00/01_project_manifest_and_tooling_lock.md](features/00/01_project_manifest_and_tooling_lock.md)
- [features/01_data_and_service_layer.md](features/01_data_and_service_layer.md)
- [features/01/00_profile_field_list_and_versions.md](features/01/00_profile_field_list_and_versions.md)
- [features/01/01_data_contracts_and_schemas.md](features/01/01_data_contracts_and_schemas.md)
- [features/01/02_profile_save_session_services.md](features/01/02_profile_save_session_services.md)
- [features/01/03_remote_config_and_analytics.md](features/01/03_remote_config_and_analytics.md)
- [features/02_animation_pipeline.md](features/02_animation_pipeline.md)
- [features/02/00_rig_source_and_naming.md](features/02/00_rig_source_and_naming.md)
- [features/02/01_rig_retargeting_and_locomotion.md](features/02/01_rig_retargeting_and_locomotion.md)
- [features/02/02_batting_animation_set.md](features/02/02_batting_animation_set.md)
- [features/02/03_bowling_animation_set.md](features/02/03_bowling_animation_set.md)
- [features/02/04_fielding_keeper_and_presentation.md](features/02/04_fielding_keeper_and_presentation.md)
- [features/02/05_animator_timeline_and_events.md](features/02/05_animator_timeline_and_events.md)
- [features/03_scene_and_ui_pipeline.md](features/03_scene_and_ui_pipeline.md)
- [features/03/00_ui_design_tokens_and_hud_layout.md](features/03/00_ui_design_tokens_and_hud_layout.md)
- [features/03/01_boot_login_and_home_flow.md](features/03/01_boot_login_and_home_flow.md)
- [features/03/02_tutorial_nets_and_match_setup.md](features/03/02_tutorial_nets_and_match_setup.md)
- [features/03/03_match_hud_replay_and_results.md](features/03/03_match_hud_replay_and_results.md)
- [features/03/04_customize_and_online_lobby.md](features/03/04_customize_and_online_lobby.md)
- [features/03/05_audio_haptics_and_feedback.md](features/03/05_audio_haptics_and_feedback.md)
- [features/04_core_cricket_gameplay.md](features/04_core_cricket_gameplay.md)
- [features/04/00_ball_state_and_timing_table.md](features/04/00_ball_state_and_timing_table.md)
- [features/04/01_ball_physics_and_contact_model.md](features/04/01_ball_physics_and_contact_model.md)
- [features/04/02_batting_outcome_and_shot_model.md](features/04/02_batting_outcome_and_shot_model.md)
- [features/04/03_bowling_and_fielding_flow.md](features/04/03_bowling_and_fielding_flow.md)
- [features/04/04_rules_engine_and_match_state.md](features/04/04_rules_engine_and_match_state.md)
- [features/05_camera_input_tutorial_flow.md](features/05_camera_input_tutorial_flow.md)
- [features/05/00_action_maps_and_gesture_thresholds.md](features/05/00_action_maps_and_gesture_thresholds.md)
- [features/05/01_camera_modes_and_presentation.md](features/05/01_camera_modes_and_presentation.md)
- [features/05/02_touch_and_controller_input.md](features/05/02_touch_and_controller_input.md)
- [features/05/03_tutorial_and_first_run_flow.md](features/05/03_tutorial_and_first_run_flow.md)
- [features/06_mobile_mvp_and_progression.md](features/06_mobile_mvp_and_progression.md)
- [features/06/00_mobile_mvp_contract.md](features/06/00_mobile_mvp_contract.md)
- [features/06/01_mobile_session_loop.md](features/06/01_mobile_session_loop.md)
- [features/06/02_progression_rewards_and_unlocks.md](features/06/02_progression_rewards_and_unlocks.md)
- [features/06/03_mobile_performance_and_settings.md](features/06/03_mobile_performance_and_settings.md)
- [features/06/04_progression_numbers_and_unlock_table.md](features/06/04_progression_numbers_and_unlock_table.md)
- [features/06/05_device_tier_budget.md](features/06/05_device_tier_budget.md)
- [features/07_multiplayer_readiness.md](features/07_multiplayer_readiness.md)
- [features/07/00_networking_go_no_go_and_budget.md](features/07/00_networking_go_no_go_and_budget.md)
- [features/07/01_authority_and_replication_model.md](features/07/01_authority_and_replication_model.md)
- [features/07/02_lobby_and_session_flow.md](features/07/02_lobby_and_session_flow.md)
- [features/07/03_disconnect_reconnect_and_backfill.md](features/07/03_disconnect_reconnect_and_backfill.md)
- [features/07/04_authority_matrix_and_replication_table.md](features/07/04_authority_matrix_and_replication_table.md)
- [features/08_testing_ci_and_release.md](features/08_testing_ci_and_release.md)
- [features/08/00_ci_toolchain_and_commands.md](features/08/00_ci_toolchain_and_commands.md)
- [features/08/01_edit_and_play_mode_tests.md](features/08/01_edit_and_play_mode_tests.md)
- [features/08/02_mobile_validation_and_performance.md](features/08/02_mobile_validation_and_performance.md)
- [features/08/03_ci_and_release_hardening.md](features/08/03_ci_and_release_hardening.md)
- [features/08/04_release_gates_and_observability.md](features/08/04_release_gates_and_observability.md)

## Multiplayer and Architecture

- [TDD.md](TDD.md)
- [competitive_analysis.md](competitive_analysis.md)
- [risk_resolution.md](risk_resolution.md)
- [risks.md](risks.md)
- [licensing_notes.md](licensing_notes.md)

## Data Schemas

- [mode_config_schema.json](mode_config_schema.json)
- [player_schema.json](player_schema.json)
- [ground_schema.json](ground_schema.json)
- [pitch_schema.json](pitch_schema.json)
- [delivery_physics_constants.json](delivery_physics_constants.json)
