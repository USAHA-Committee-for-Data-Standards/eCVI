# eCVI Repository Transfer Record

**Date of transfer:** September 28, 2026

## Summary

On September 28, 2026, the eCVI repository was transferred from the personal GitHub account **AAVLD-USAHA-ITStandards** to the organization account **USAHA-Committee-for-Data-Standards**. All three committee repositories are now stored in the organization account.

## Background

- The eCVI repository was created on August 30, 2017, in a personal GitHub account named AAVLD-USAHA-ITStandards. That account was set up in the name of the standard using a dedicated Google account.  
- The organization account USAHA-Committee-for-Data-Standards was created on November 3, 2023\.  
- At an earlier date, the Permit repository and the AAVLD-USAHA-ITStandards repository (a README with links to the eCVI and Permit repositories) were moved from the personal account to the organization account. The eCVI repository was not moved at that time.  
- A personal account is controlled by a single login. An organization account is managed through admin roles held by individual members, so control does not depend on one password.

## Actions taken

1. Confirmed through GitHub's records that AAVLD-USAHA-ITStandards is a personal account and that the eCVI repository was still stored in it.  
2. Recovered access to the personal account through its Google account, with help from Dr. Michael Martin and Dr. Gustavo Machado.  
3. Transferred the eCVI repository to USAHA-Committee-for-Data-Standards (September 28, 2026).  
4. Verified the transfer in GitHub's records.  
5. Added zaluskim (Dr. Marty Zaluski) as an admin of the organization account.

## Results

- **New address:** [https://github.com/USAHA-Committee-for-Data-Standards/eCVI](https://github.com/USAHA-Committee-for-Data-Standards/eCVI)  
- **Old links still work.** Links to [https://github.com/AAVLD-USAHA-ITStandards/eCVI](https://github.com/AAVLD-USAHA-ITStandards/eCVI) (including folders, issues, and pull requests) redirect automatically to the new address.  
- **Carried over:** full commit history, branches, tags, releases, issues, pull requests, stars, watchers, and forks.

## Structure before the transfer

```
ACCOUNT: USAHA-Committee-for-Data-Standards   (organization)
│   Admins: gustavo-etal (Gustavo Machado)
│           ryanscholzdvm (Dr. Scholz)
│           AAVLD-USAHA-ITStandards (the personal account below)
├── REPO: Permit                    (moved here from the personal account)
└── REPO: AAVLD-USAHA-ITStandards   (README with links to eCVI and Permit, moved here from the personal account)

ACCOUNT: AAVLD-USAHA-ITStandards              (personal)
│   Admin: whoever holds the login
└── REPO: eCVI                      (XML standard files)
```

## Structure after the transfer

```
ACCOUNT: USAHA-Committee-for-Data-Standards   (organization)
│   Admins: gustavo-etal (Gustavo Machado)
│           ryanscholzdvm (Dr. Scholz)
│           zaluskim (Dr. Zaluski)
│           AAVLD-USAHA-ITStandards (the personal account below)
├── REPO: eCVI                      (XML standard files, moved here Sep 28, 2026)
├── REPO: Permit                    (moved here from the personal account)
└── REPO: AAVLD-USAHA-ITStandards   (README with links to eCVI and Permit, moved here from the personal account)

ACCOUNT: AAVLD-USAHA-ITStandards              (personal)
└── (no standards repositories)
```

## Notes for maintainers

- **Do not create a repository named "eCVI" in the AAVLD-USAHA-ITStandards personal account.** Doing so would break the redirect from the old address.  
- **Do not delete the AAVLD-USAHA-ITStandards personal account.** Keeping it prevents anyone else from registering that name and breaking the old links. Its login is held by the committee.  
- The organization profile page (repository `.github`, file `profile/README.md`) is the current table of contents for the committee's standards.