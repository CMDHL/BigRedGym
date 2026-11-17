# Assignment 6 submission steps

1. Review the generated outline PDF and the two Markdown drafts.
2. Record the autonomous MuJoCo demo from `D:\MAE5110\HuaijinPeter\BigRedGym`:

   ```powershell
   uv run --frozen python scripts/play.py --task go2height --original_cfg --load_run Oct16_18-01-11_height_v1 --checkpoint 500 --num_envs 1 --device cpu --backend mujoco --demo-forward-height --demo-speed 0.2 --demo-walk-seconds 3 --demo-stop-seconds 1 --demo-crouch-seconds 2 --no-keyboard
   ```

   Close the MuJoCo viewer after the useful sequence is recorded.

3. Create and push a review branch only when ready:

   ```powershell
   git switch -c assignment-6-go2-composition
   git add gym scripts tests deliverables
   git commit -m "Evaluate Go2 height-control readiness"
   git push -u origin assignment-6-go2-composition
   ```

4. Open this comparison, choose **Create draft pull request**, and paste `draft_pr_description.md`:

   `https://github.com/LampLighterLab/BigRedGym/compare/main...HuaijinPeter:BigRedGym:assignment-6-go2-composition?expand=1`

5. Replace `<DRAFT_PR_URL>` and the recording placeholder in `ed_discussion_post.md`. Create the Ed Discussion post, paste the Markdown, and attach/link the recording and final PDF. Explicitly state that the current controller is not hardware-ready and ask course staff for the Go2Trot checkpoint.
6. Upload `output/pdf/assignment_6_project_outline.pdf` to the Assignment 6 submission page before 11:59 PM tonight.
