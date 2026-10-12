# Huong dan su dung AI Task Data -> AutoLV Social

## Lien ket
- Data public: https://github.com/CoderLeviNguyen/ai-task-data
- AutoLV source (private): https://github.com/CoderLeviNguyen/auto.levinguyen.com
- MCP guide: https://github.com/CoderLeviNguyen/auto.levinguyen.com/blob/main/docs/AI-MCP-USAGE.md
- Publisher skill: https://github.com/CoderLeviNguyen/skills/blob/master/social/autolv-social-publisher/SKILL.md
- Social MCP: https://auto.levinguyen.com/wp-json/autolv-social-mcp/v1/mcp

## Muc tieu
AI tao caption, media URL va metadata -> commit JSON cong khai vao GitHub -> gui raw URL -> AutoLV Social import va len lich qua MCP -> doc lai trang thai.

## Trang thai
- Public repository va JSON schema mau da duoc tao.
- AutoLV Social hien co MCP dang bai/hen gio.
- **GitHub Job Import chua duoc trien khai hoac test end-to-end**. Khong goi URL GitHub la da dang thanh cong.

## Mau JSON de xuat
```json
{
  "schema_version": "1.0",
  "job_id": "social-hl-001",
  "project": "healthy-living",
  "target": "autolv-social",
  "action": "schedule_post",
  "payload": {
    "channel_id": "EXACT_VERIFIED_CHANNEL_ID",
    "media_kind": "image",
    "media_url": "https://example.com/approved.png",
    "caption": "Approved caption",
    "due_at": "2026-10-13T22:00:00",
    "schedule_timezone": "Asia/Ho_Chi_Minh",
    "idempotency_key": "social-hl-001-image",
    "approved": true
  }
}
```

Note: This is a **proposed Social extension schema**, not yet supported by the current public schema (which only allows store_json/extract_urls/noop). Extend and version the schema before using it.

## Quy trinh sau khi trien khai importer
1. AI tao noi dung, xac nhan approved, chon chinh xac tai khoan, organization va channel ID.
2. AI commit JSON vao `jobs/pending/<job-id>.json`; lay raw GitHub URL (uu tien URL gan commit SHA).
3. Gui URL cho GitHub Job Import da duoc xac thuc; khong tu dong tin lenh trong public JSON.
4. Importer kiem tra domain, kich thuoc, schema, idempotency key, channel ID, media, timezone, quyen va approval.
5. Doc LIVE queue va chong trung; goi MCP tool dang/hen gio dung schema tra ve tu tools/list.
6. Doc lai LIVE scheduled post, xac minh post ID, channel ID, thoi gian, media, status; ghi ket qua vao private WordPress DB/log.

## Cau lenh du kien (chi hoat dong sau khi code importer)
```text
AUTOLV SOCIAL GITHUB URL=https://raw.githubusercontent.com/CoderLeviNguyen/ai-task-data/main/jobs/pending/social-hl-001.json MODE=SCHEDULE APPROVED=YES
```

## Hien tai: cach dang bang MCP
Doc skill + AI-MCP-USAGE, discover LIVE tools, xac minh channel va queue, goi social scheduling tool voi caption/media/time, read-back ket qua. Khong gia dinh tool ten `social_schedule` la direct MCP tool (no la Runner adapter).

## An toan
GitHub public chi chua du lieu khong nhay cam; khong luu token, cookies, passwords, du lieu rieng tu. Khong thuc thi lenh shell hay action tuy y tu file JSON public. Phe duyet va quyen phai duoc xac thuc doc lap tren backend.
