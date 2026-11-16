# ♿ Accessibility and API consumer usability

## Why accessibility belongs in a backend project

A gateway may not render a user interface, but it shapes the messages, developer workflows, examples, and operational tools that people use. Poorly designed errors and inaccessible documentation make the API harder to integrate and support.

## 1. API error usability

- Use stable error codes that clients can branch on.
- Keep messages plain, specific, and actionable.
- Do not expose private account existence or internal authorization rules.
- Provide field-level validation details only when safe.
- Document 429 retry semantics.
- Include a correlation ID for support without exposing user identity.
- Keep error structure consistent across services.

## 2. Documentation accessibility

- Maintain a logical heading hierarchy.
- Use descriptive link text.
- Add alt text to every meaningful image.
- Explain complex diagrams in surrounding prose.
- Do not rely on colour or emoji alone to communicate severity.
- Use readable tables and concise column labels.
- Provide language labels for code fences.
- Keep examples copyable and use synthetic values.

## 3. Future operator interface requirements

If a gateway dashboard or admin UI is introduced:
- All functionality must be available by keyboard.
- Every input must have a persistent label.
- Focus indicators must be visible.
- Focus order must follow the visual and task order.
- Dialogs must manage focus and escape behavior.
- Status updates must be exposed to assistive technology.
- Tables must use semantic headers.
- Contrast must meet the selected accessibility standard.
- Zoom and reduced-motion preferences must be respected.

## 4. Review checklist

- [ ] Error codes are documented and stable.
- [ ] Error messages avoid unnecessary jargon.
- [ ] Images have useful alt text.
- [ ] Mermaid diagrams have textual explanation.
- [ ] Heading order is logical.
- [ ] Links describe their destination.
- [ ] Any future UI is keyboard accessible.
- [ ] Status does not rely on colour alone.

## 5. Standards

Use the current W3C Web Content Accessibility Guidelines as the reference for any user-facing interface. API error and documentation conventions should also be reviewed with client developers and support staff.
