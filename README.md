# PlayerController
The Player Controller for my dungeon krawler character

This is the code for my character controller 







using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerController : MonoBehaviour
{
    public Animator Anim;

    public float stamina;
    public int gold;
    public float PlayerHP = 100;
    public int HPpotions;
    public int DungeonKey;
    public Text StaminaTxt;
    public Text Gold;
    public Text PlayerHPTXt;
    public Text potions;
    public Text KeysTxt;
    public SkeletonAi skeletonAi;
    public Canvas Pause;
    public Canvas OptionsMenu;
    public Canvas PlayerDied;
    public Light PlayerLight;
    public ParticleSystem HpParticles;
    public Rigidbody PlayerRb;
    public ChestScript Chest;
    public FirstDungeonChest firstdungeonChest;
    public OpenBigDungeon BigDungeon;

    public float AttackTimer = 3;

    public bool CanAttackAgain;
    public bool isWalking;
    public bool isIdle;
    public bool isRunning;
    public bool isAttacking;
    public bool isKicking;
    public bool outOfStamina;
    public bool isSideSteppingSideOp;
    public bool InItemRange;
    public bool isSitting;
    public bool isGrounded;
    public bool ChestIsOpen;

    void Start()
    {
        stamina = 100;
        gold = 0;
        Time.timeScale = 1;
        Pause.enabled = false;
        OptionsMenu.enabled = false;
        PlayerLight.enabled = false;
        ChestIsOpen = false;
    }

    // Update is called once per frame
    void Update()
    {
        PlayerMove();
        PlayerRun();
        Attack();
        Kick();
        RegenStamina();
        DrinkHPPotion();
        PauseGame();
        TurnOnAndOffLight();
        SitIdle();
        Jump();
        HPControl();
        OpenChest();
        UnlockBigDungeon();

        StaminaTxt.text = "Stamina : " + (int)stamina;
        Gold.text = "Gold : " + (int)gold;
        PlayerHPTXt.text = "Health : " + (int)PlayerHP;
        potions.text = "" + HPpotions;
        KeysTxt.text = "Keys : " + DungeonKey;
        AttackTimer -= 1 * Time.deltaTime;
    }

    public void PlayerMove()
    {
        if (Input.GetKey(KeyCode.W))
        {
            transform.Translate(Vector3.forward * 2 * Time.deltaTime);
            Anim.SetBool("IsWalking", true);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSitting = false;
            isSideSteppingSideOp = false;
        }
        else if (Input.GetKey(KeyCode.S))
        {
            transform.Translate(Vector3.back * 1 * Time.deltaTime);
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", true);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if (Input.GetKey(KeyCode.D))
        {
            transform.Translate(Vector3.right * 0.5f * Time.deltaTime);
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", true);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if (Input.GetKey(KeyCode.A))
        {
            transform.Translate(Vector3.left * 0.5f * Time.deltaTime);
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", true);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = false;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSitting = false;
            isSideSteppingSideOp = true;
        }
        else if(isWalking && isAttacking)
        {
            transform.Translate(Vector3.left * 0.5f * Time.deltaTime);
            Anim.SetBool("IsWalking", true);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSitting = false;
            isSideSteppingSideOp = false;
        }
        else
        {
            transform.Translate(Vector3.zero);
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", true);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = true;
            isWalking = false;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSitting = false;
            isSideSteppingSideOp = false;
        }
    }

    public void PlayerRun()
    {
        if(Input.GetKey(KeyCode.W) && Input.GetKey(KeyCode.LeftShift) && isWalking && !outOfStamina)
        {
            transform.Translate(Vector3.forward * 4 * Time.deltaTime);
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isRunning", true);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = false;
            isRunning = true;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
            stamina -= 10 * Time.deltaTime;
        }
        else if(Input.GetKey(KeyCode.W) && Input.GetKeyUp(KeyCode.LeftShift) && isRunning && !outOfStamina)
        {
            Anim.SetBool("IsWalking", true);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if(Input.GetKey(KeyCode.W) && Input.GetKey(KeyCode.LeftShift) && isRunning && outOfStamina)
        {
            Anim.SetBool("IsWalking", true);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
    }

    public void Jump()
    {
        if (Input.GetKey(KeyCode.Space) && isGrounded)
        {
            PlayerRb.AddForce(Vector2.up * 0.5f, ForceMode.Impulse);
        }
    }

    public void Attack()
    {

        if(Input.GetKeyDown(KeyCode.Mouse0) && isIdle && !outOfStamina)
        {
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", true);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = false;
            isRunning = false;
            isAttacking = true;
            isKicking = false;
            isSideSteppingSideOp = false;
            CanAttackAgain = false;
            isSitting = false;
        }
        else if (Input.GetKeyDown(KeyCode.Mouse0) && isIdle && !outOfStamina)
        {
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", true);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = true;
            isWalking = false;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if(Input.GetKeyDown(KeyCode.Mouse0) && isWalking && outOfStamina)
        {
            Anim.SetBool("IsWalking", true);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = false;
            isWalking = true;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if(Input.GetKeyUp(KeyCode.Mouse0) && isIdle && !outOfStamina)
        {
            isAttacking = false;
        }
    }

    public void Kick()
    {
        if(Input.GetKeyDown(KeyCode.Mouse1) && isIdle && !outOfStamina)
        {
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", true);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", false);
            isIdle = true;
            isWalking = false;
            isRunning = false;
            isAttacking = false;
            isKicking = true;
            isSideSteppingSideOp = false;
            isSitting = false;
        }
        else if(Input.GetKeyUp(KeyCode.Mouse1) && isIdle && !outOfStamina)
        {
            isKicking = false;
        }

    }

    public void RegenStamina()
    {
        if(stamina > 0)
        {
            if (stamina < 100 && !isRunning && !isKicking && !isAttacking)
            {
                stamina += 10 * Time.deltaTime;
            }
            else if (stamina < 1)
            {
                outOfStamina = true;
            }

            if (stamina < 30)
            {
                StaminaTxt.color = Color.red;
            }



            if (stamina > 10)
            {
                outOfStamina = false;
            }

            if (stamina > 15)
            {
                StaminaTxt.color = Color.blue;
            }
         }
    }

    public void DrinkHPPotion()
    {
        if(HPpotions > 0 && PlayerHP < 100)
        {
            if (Input.GetKeyDown(KeyCode.Alpha1) && isIdle)
            {
                PlayerHP += Random.Range(1,20);
                Instantiate(HpParticles, transform.position, Quaternion.Euler(new Vector3(-90, 0, 0)));
                HPpotions--;
            }
        }
    }

    public void HPControl()
    {
        if(PlayerHP > 101)
        {
            PlayerHP--;
        }
    }

    public void PauseGame()
    {
        if (Input.GetKey(KeyCode.Escape))
        {
            Pause.enabled = true;
            Time.timeScale = 0;
            Cursor.lockState = CursorLockMode.None;
        }
    }

    public void SitIdle()
    {
        if (Input.GetKey(KeyCode.V) && isIdle)
        {
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", true);
            isIdle = false;
            isWalking = false;
            isRunning = false;
            isAttacking = false;
            isKicking = false;
            isSideSteppingSideOp = false;
            isSitting = true;

            if(PlayerHP < 80 && isSitting)
            {
                PlayerHP += 1 * Time.deltaTime;
            }
        }
    }

    public void TurnOnAndOffLight()
    {
        if (Input.GetKeyDown(KeyCode.E) && !PlayerLight.enabled)
        {
            PlayerLight.enabled = true;
        }
        else if(Input.GetKeyDown(KeyCode.E) && PlayerLight.enabled)
        {
            PlayerLight.enabled = false;
        }
    }

    public void OpenChest()
    {
        if (Chest.PlayerInRange && Input.GetKeyDown(KeyCode.F))
        {
            Chest.Anim.SetBool("isOpen", true);
            Chest.DropLoot();
        }
        else if (firstdungeonChest.inRange && Input.GetKeyDown(KeyCode.F))
        {
            firstdungeonChest.Anim.SetBool("isOpen", true);
            firstdungeonChest.DropDungeonLoot();
        }
    }

    public void UnlockBigDungeon()
    {
        if(BigDungeon.CanOpenDungeon && Input.GetKeyDown(KeyCode.F) && DungeonKey > 0)
        {
            Destroy(BigDungeon.gameObject);
            DungeonKey -= 1;
        }
    }


    public void OnCollisionEnter(Collision collision)
    {
        if (collision.transform.CompareTag("Item"))
        {
            Destroy(collision.gameObject);
            gold += Random.Range(1,50);
        }
        else if (collision.transform.CompareTag("HPPotion"))
        {
            Destroy(collision.gameObject);
            HPpotions += Random.Range(1, 4);
        }
        else if (collision.transform.CompareTag("Skeleton"))
        {
            Anim.SetBool("IsWalking", false);
            Anim.SetBool("isIdle", false);
            Anim.SetBool("isSideStepping", false);
            Anim.SetBool("isWalkingBack", false);
            Anim.SetBool("isRunning", false);
            Anim.SetBool("isPunching", false);
            Anim.SetBool("isKicking", false);
            Anim.SetBool("isSideSteppingOp", false);
            Anim.SetBool("isSitting", false);
            Anim.SetBool("HasBeenHit", true);

            PlayerRb.AddForce(Vector3.back * 4, ForceMode.Impulse);
        }
        else if (collision.transform.CompareTag("Ground"))
        {
            isGrounded = true;
        }

    }

    public void OnCollisionExit(Collision collision)
    {
        if (collision.transform.CompareTag("Ground"))
        {
            isGrounded = false;
        }
    }
}
