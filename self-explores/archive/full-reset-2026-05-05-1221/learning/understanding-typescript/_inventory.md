---
type: course-inventory
course_slug: understanding-typescript
created: 2026-05-04
---

# Course Inventory

## Folder Paths

- Course root: /home/admin88/2_learning/type-script/typescript
- Lectures folder: lectures/
- Resources folder: resources/
- TOC file: lectures.txt
- Repo folder: không có (resources/ chứa code starter/finished của từng bài)

## Counts

| Type | Count | Notes |
|---|---:|---|
| Transcript files | 344 | Trong lectures/, đánh số theo lecture ID |
| Resource folders | 92 | Trong resources/, mỗi folder là code của 1+ lectures |
| Lecture trong TOC | 339 | lectures.txt — source of truth về thứ tự |

## Naming Patterns

### Lecture File Pattern
- Format: `{N}.Lecture_Title_With_Underscores.txt`
- Example: `15.Type_Inference_vs_Type_Assignment.txt`
- ID extraction: number trước dấu chấm đầu tiên

### Resource Folder Pattern
- Format: `{N}-{description}` hoặc `{N}-{section}-{description}`
- Example: `147-decorators-02-first-class-decorator`
- ID mapping: số đầu → lecture ID tương ứng
- Cấu trúc bên trong: `{folder-name}/{inner-folder}/src/app.ts` + `tsconfig.json` + `package.json`

### Code Pattern
- Main language: TypeScript
- Main files: `src/app.ts` trong mỗi resource folder
- Config: `tsconfig.json`, `package.json`
- Entry points: `src/app.ts`

## Data Quality

| Issue | Severity | Notes |
|---|---|---|
| Lectures 258–339 trùng nội dung | medium | Legacy lectures — có thể SKIP toàn bộ |
| Resources chỉ có cho ~25% lectures | low | Chỉ lectures có code mới có resource folder |
| Một số resource folders không map lecture cụ thể | low | VD: resources/3-demo-app |
